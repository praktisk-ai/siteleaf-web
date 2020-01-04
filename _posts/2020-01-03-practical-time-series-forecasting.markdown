---
title: Practical time series forecasting
date: 2020-01-03 19:43:00 +01:00
image: "/uploads/cball.jpg"
---

Time series forecasting shows up in many different problems. It could be to forecast sales of a specific product, total sales for a whole chain of stores, product price changes, electricity consumption, volume of car rentals, visitors to a store, etc. 

In this article I will discuss how to work through a time series forecasting problem from a practical point of view. The reader is expected to know the principles of AI/ML and to be extremely interested in time series forecasting! 

**What is a time series?**

In this article when I discuss a time series I mean a list of rows where each row is a date and an associated value. It looks like this:

1972-05,	4618

1972-06,	5312

1972-07,	4298

1972-08,	1413

1972-09,	5877


**Predictability**

A big problem when you have a completely new dataset is how to know if it is even possible to get a good prediction. Is there some structure and pattern in this dataset? How much time and effort should you put into the “research” of it? Well, for publicly available datasets there is sometimes someone else who has already done some analysis and written about it. Kaggle is another good source. However these published metrics are not as common as you might expect...

So how do we know how good we can do on a completely new dataset? Well, we start out by calculating the simplest possible accuracy, which are the naive baselines (see below). Then we move on to use off-the-shelf products and services - how good can someone else do with this data in a standardized way? Then finally, if needed, we could go on to do some private dataset research to see if we can get even better numbers than the found baselines.

**Champagne**

When doing experimentation on new algorithms or services I like to use a known dataset as explained above. This way I know how good the accuracy can be for the dataset. Here I will use a dataset about champagne sales, because champagne is nice! 

(You can get the dataset here: https://raw.githubusercontent.com/jbrownlee/Datasets/master/monthly_champagne_sales.csv
)

![champ.png](/uploads/champ.png)



**Dataset parts**

In this article I will split the dataset into three parts: training, validation and test. The training set is used to train the model, validation set is used to see how well the training worked and for tuning the training. The test set is only used at the very end to verify the final model, so I will not discuss it much here.



It is quite often important exactly what data to use for training vs validation vs test, since we want a validation set that represents the test set, which in turn should represent the real world. For time series the most important thing to consider is to use the most recent data as the test dataset, and the next most recent data as the validation dataset. Mixing this up into the training dataset would be a bit like having access to data from the future, which would probably make your model predict worse in a production environment.

**Metrics**

I will use a simple metric in this article called RMSE, Root Mean Squared Error. It basically says this: how big is the error (on average) for a specific prediction.

**Simple baselines**

Every baseline is like a new personal best for the dataset - the accuracy number you have to beat with your next approach. If you can’t even beat the baseline, then you have a problem. Perhaps there is no structure in the data? Perhaps it is just a random walk series with no predictability at all? These are some simple baselines to start with:

* Mean
* Median
* Previous value
* Baseline from article

So running some simple calculations we find these metrics:

* Mean, RMSE=2967
* Median, RMSE=3170

![b1-ddfa6d.png](/uploads/b1-ddfa6d.png)

* Previous value, RMSE=3186

![b2.png](/uploads/b2.png)


These baselines are off by about 40%, which is not great. 

Now it just so happens that there is an article about this dataset which gives us a better number to beat. The method used in the article is ARIMA which achieves RMSE=924 on the validation which is really a big step forward compared to the simple baseline. This number is only about 10-15% wrong for a prediction. From these few simple steps we now know what ballpark numbers are achievable and what we have to beat. Let’s move on to the more advanced baselines!

(Article: https://machinelearningmastery.com/time-series-forecast-study-python-monthly-sales-french-champagne/ )


**Advanced baselines**

Now we will have to do some more work to get some even better baselines, but still not a massive amount. We just reuse the frameworks and tools that others have provided for us to do the heavy lifting - maybe they will even be good enough for our business problem? If not, at least we will get a sense for how good we should be able to get with some more work.

* FB Prophet
* AWS Forecast
* AWS SageMaker Studio

**Prophet**

Prophet is a nice forecasting framework from Facebook (https://facebook.github.io/prophet/) that in a few lines of code and very fast gets you yet another baseline. For this dataset I got RMSE=1500. So much better than the simple baselines but not as good as the one from the article. This is normally to be expected since someone has actually done some manual tweaking to get the number on the article. But say we did not have the article - then we would now have cut the other simple baselines in half (from about 3000). So this is still a good tool to know about and try out.

![prophet.png](/uploads/prophet.png)

**AWS Forecast**

AWS has a long tradition of using ML and especially forecasting for their large product catalog. They also provide this functionality as a separate service called AWS Forecast (https://aws.amazon.com/forecast). Just create a datafile that you upload to S3 and then you can start training and generating predictions. One note about this service though - it is kind of slow. My guess is that it spins up AMIs for all activities in the background. Apart from that this is a good remote service and resembles SageMaker Studio in a lot of conceptual ways.

I allowed AWS FC to pick algorithm itself and it chose ARIMA among 5 different ones tested. The achieved performance with default settings yielded RMSE=1030. Better than Prophet and almost as good as the handcoded ARIMA from the article. So also a quite good result here.

This is what the AWS FC predictions look like:

![fc-pred.png](/uploads/fc-pred.png)


**AWS SageMaker Studio**

Studio provides a nice framework for automating the hyperparameter and algorithm search process. At this time however I did not find any support for time series analysis. This will probably be included in the future.


**Custom dataset research**

Let us now assume that we are not satisfied with the results from the baselines and we want more. We think that we could probably do better ourselves. What to do?

**Keras Neural Nets**

Keras is a framework that provides some nice high-level constructs on top of e.g. TensorFlow. You will still have to know some basic things about how to create NNs though. But mostly you can stick to a code template to create something usable. 

Now starts the creative phase where you have to come up with features like lag, date information, moving averages, trends, etc. The big promise of NNs are that they should *not* require a great deal of feature engineering, but should discover the patterns themselves. However, they need a bit of TLC before they actually do what you want. For instance the data sometimes need normalization to work optimally. You also need to find a network architecture that has the ability to capture whatever patterns your data is hiding. How many layers should you use? How many hidden nodes?

Contrast this with e.g. RandomForests where you normally do no preprocessing of the data at all - and you can still achieve impressive results. Although RFs still require creative features to have something to work with - the more the better! 

After this you have to run hyperparameter search across the knobs in your NN that you think are important, like layers, training time, features to include, etc. 

I used only a basic NN for this example with one and two hidden layers including Relu activation layers after each. The best I could get here was RMSE=834. 


**Fast.ai Neural Nets**

Fast.ai also provides a high-level library, and with up to date implementations of almost every new idea that pops up in the AI community. Currently it sits on top of PyTorch. So using this library you get a lot of functionality for free, especially when it comes to optimizing and improving NN training. This includes learning rate, dropouts, categorical features, momentum, batch normalization, etc. It also gives you the ability to tweak them all!

There is a bit more setup to do when using the fast.ai libraries, but it is worth it as we shall see.

Hyperparameter searching can become a big task using fast.ai because of all the tweakable parameters. This is a good thing but can be a bit overwhelming. You just have to start tweaking and eventually you will get a feeling for what seems to be good parameters. Or you just code up the resource intensive hypersearch right away and kick it off. About feature engineering, I kept it to a minimum and used only two years of lag data and the current month as features. 

What I finally ended up with here was RMSE=426, which translates to about 8% error for a prediction. Pretty nice! This is what it looks like:

![fastai2.png](/uploads/fastai2.png)



**Summary**

We started out by our simple baselines with RMSE around 3000. Got the free one from the article of 924, but this will obviously not happen in the real world. Then we used a few off-the-shelf products to get a feel for how good we could get the predictions and landed on 1500 from Prophet and 1030 from AWS FC. A simple NN gave us 834, and when the smoke cleared fast.ai was the winner with 426.

Of course the real trial by fire of the chosen model comes when we evaluate it on the test set, i.e. the hold-out data the model has not yet seen. But that is another story... Anyway, now we won’t be losing any more sleep over future sales. Bring the champagne!



Torbjörn Stavenek

