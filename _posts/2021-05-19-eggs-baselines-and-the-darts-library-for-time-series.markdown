---
title: Eggs, baselines and the Darts library for time series
date: 2021-05-19 21:11:00 +02:00
image: "/uploads/egg-ring.jpeg"
---

I have some chickens: 6 hens and 2 roosters. So I get a lot of eggs. I have written down how many eggs I get every day for some time now to see if there are any patterns in the production.
In this article I will look at the value of a baseline for your AI problem as well as taking a quick look at the Darts library.


# Importing data

Normally a hen does not lay an egg every day since it takes between 20-30 hours for an egg to be completed. But I am thinking that my hens probably use the same production time for every new egg and that this must show up in the data.

This is basically a time series forecasting problem. So we should be able to apply the normal time series methods. However, since the amount of eggs produced is so small, 0-6, and discrete ie only whole numbers, it could also be considered a classification problem.
So I will look at this from both angles using some cool libraries.

darts is a Python library for easy manipulation and forecasting of time series. It contains a variety of models, from classics such as ARIMA to deep neural networks. The models can all be used in the same way, using fit() and predict() functions, similar to scikit-learn.


Import the data into a normal Pandas dataframe and plot it.

 
    df = pd.DataFrame(egg_data, columns=["Eggs"])
    df['Day'] = pd.date_range(start='2021-01-01', periods=len(egg_data))
    print(df)
    df.plot(x='Day', figsize=(15,5))

![Skärmavbild 2021-05-19 kl. 20.45.01.png](/uploads/Ska%CC%88rmavbild%202021-05-19%20kl.%2020.45.01.png)


Now import into a darts object:

 
    series = TimeSeries.from_dataframe(df, 'Day', ['Eggs'])
    series.plot()


Split the time series into separate datasets for training and validation. We use 70% for training.

    split_day = df.Day.iloc[round(len(df.Day) * 0.70)]
    train, val = series.split_before(split_day)
    train.plot(label='training')
    val.plot(label='validation')
    plt.legend()

![Skärmavbild 2021-05-19 kl. 20.48.20.png](/uploads/Ska%CC%88rmavbild%202021-05-19%20kl.%2020.48.20.png)


# Baseline

Now let us create a baseline. What is a baseline? Well it is the most basic prediction you can do that you then try to beat with your mlore advanced algorithms. Here we will use the average and calculate the Mean Average Error (MAE).

    avg = train.mean().values[0]
    m_mae = sum(abs(val.values() - avg)) / len(val)
    print(avg, m_mae)

    2.4183673469387754 [0.7585034]

So we can see that guessing the average number of eggs per day based on the training data, 2.4, we get an error on average of 0.8 eggs.

    q = pd.DataFrame(val.values(), columns=['val'])
    q.plot(figsize=(20,5))
    plt.axhline(avg, color='r')

![Skärmavbild 2021-05-19 kl. 20.51.30.png](/uploads/Ska%CC%88rmavbild%202021-05-19%20kl.%2020.51.30.png)


But it seems a bit strange to make a guess of non-whole eggs, so we round it down to 2. Then we get this.

    avg = round(train.mean().values[0])
    m_mae = sum(abs(val.values() - avg)) / len(val)
    print(avg, m_mae)

    2 [0.61904762]


![Skärmavbild 2021-05-19 kl. 20.54.22.png](/uploads/Ska%CC%88rmavbild%202021-05-19%20kl.%2020.54.22.png)

Which is actually better than the “decimal guess”, now the model guess only 0.62 eggs wrong for a particular day!

# More advanced stuff

Let us first take a look at the auto-correlation between the datapoints (ie eggs per day).

    plot_acf(train)


![Skärmavbild 2021-05-19 kl. 20.56.55.png](/uploads/Ska%CC%88rmavbild%202021-05-19%20kl.%2020.56.55.png)

We see that we have a strong correlation with day 0 since this is what we compare to. Otherwise most obvious is the strong negative correlation with the previous day. This seems logical - if there were many eggs on day 1 then there should be less eggs the next day, and vice versa.

We can also run a statistical check of seasonality for each candidate period m.

    for m in range(2, 25):
       is_seasonal, period = check_seasonality(train, m=m, alpha=.05)
       if is_seasonal:
           print('There is seasonality of order {}.'.format(period))

    There is seasonality of order 2.
    There is seasonality of order 4.
    There is seasonality of order 7.
    There is seasonality of order 9.
    There is seasonality of order 12.
    There is seasonality of order 14.
    There is seasonality of order 17.
    There is seasonality of order 19.
    There is seasonality of order 21.

Hmm. Seasonality all over the place, which is actually most indicative of NO seasonality.
However, let us run a few different models. Note that the evaluation metric cannot be simple naive MAPE since we have zeros in the data. We need to perform variation on the calculation that works on all errors together to avoid the zero-problem.

    def eval_model(model):
       model.fit(train)
       forecast = model.predict(len(val))
 
       total_err = sum(abs(forecast.values() - val.values() ))
       total_actual = sum(val.values())
       mape =  sum(total_err/total_actual)
       mae = total_err / len(val)
       print(model,  total_err, total_actual, mape, mae)
     
       q = pd.DataFrame(val.values(), columns=[str(model) + ': val'])
       q['pred'] = forecast.values()
       q.plot(figsize=(20,5))
 
 
    eval_model(ExponentialSmoothing())
    eval_model(Prophet())
    eval_model(AutoARIMA())


    Exponential smoothing [26.55093084] [90] 0.2950103426938217 [0.63216502]
    Prophet [34.53034244] [90] 0.38367047160114776 [0.82215101]
    Auto-ARIMA [31.6753797] [90] 0.3519486633090237 [0.75417571]


![Skärmavbild 2021-05-19 kl. 21.01.15.png](/uploads/Ska%CC%88rmavbild%202021-05-19%20kl.%2021.01.15.png)

The most interesting thing here is that none of these basic statistically based models could beat our baseline! The best model was “Exponential smoothing” which had an MAE of 0.63 but that is still worse than our baseline. 


# Summary
This article did a quick exploratory data analysis (EDA) on some egg data using the darts library. We learned that we should always start with a baseline to get a feeling for what our more advanced and time consuming algorithms must beat in terms of accuracy.
  
So if we want good results for this data we need to look into more sophisticated algorithms. But that is a topic for another article. 
