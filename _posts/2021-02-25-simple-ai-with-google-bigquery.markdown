---
title: Simple AI with Google BigQuery
date: 2021-02-25 14:14:00 +01:00
image: "/uploads/bq4.png"
---

In this post I will discuss two different AI use cases built around the AI capabilities provided by Google BigQuery ML. This is a quick and easy way to get started with your AI projects, especially if you are already using Google Cloud. The first use case is a time series forecasting problem where we want to predict some monthly tracked cost 12 months ahead. The second use case is a variation of the first one where we basically want to predict the same cost for just the next month but based on a count of events that has occurred one or two months before today. This post will be a bit more technical than usual including some SQL, but still simple enough that I think everyone will be able to follow along. 

# What is BigQuery?

BigQuery is an enterprise-scale data warehouse database-as-a-service offering. It is supposedly serverless but as we use it as a service we don’t really care. BigQuery allocates storage and query resources dynamically based on your usage patterns. It is also a type of columnar database so we do not need to explicitly specify indexes to perform searches effectively. This is done for you.  You can access it using ANSI SQL, or libraries for multiple languages. One nice feature is simple access from Google Colab, Google’s free Jupyter notebooks offering.

# What is BigQuery ML?
BigQuery ML lets you train and run machine learning models inside BigQuery. You can choose among a number of provided ML-models and you configure both training and inference using an extended version of SQL queries. 

The first big advantage of this is how simple it is to create and host a model as I will discuss in the use cases below. The second advantage is that you don’t need to extract out of BigQuery all the data required for training - instead you move the training code to the data. These features increase development speed and lessen the operational burden.

Some of the models available are:

* Linear/Logistic regression
* Boosted trees classifier/regressor
* Deep neural networks classifier/regressor
* K-means
* ARIMA

# Use case 1: One year horizon cost forecasting

![arima.png](/uploads/arima.png)

In this use case we have stored a monetary cost every month so now we have a monthly time series of costs. Using this time series we want to forecast the cost every month with a one year horizon, meaning we want to forecast 12 numbers. We assume the required data is already available in BigQuery.

**Data**

Here is a data extract.

<pre>
SELECT monthDate, totalAmount FROM cost
</pre>


**Training the model**

As I mentioned before there are a number of models available in BigQuery and one of them is ARIMA which is a time series forecasting model. Now we want to create an ARIMA-model based on the data in our table, and here is the code to do that.

<pre>
 CREATE MODEL my_arima_model
 OPTIONS (
   model_type = 'ARIMA',
   time_series_timestamp_col = 'monthDate',
   time_series_data_col = 'totalAmount',
   DATA_FREQUENCY = 'MONTHLY',
   ) AS
SELECT monthDate, totalAmount
FROM ( SELECT monthDate, totalAmount FROM cost )
</pre>

When we have created this model we can also look at various parameters and metrics of the model.

One problem is that this model is never updated. So until we update it we will get the same forecasts forever… It would be natural to update this model once a month, since that is when we have one more month of data to train the model on. Fantastically enough there is a little button in the Google console named “Schedule” which allows us to schedule this query to run every month! 

So when we have specified and scheduled this query, the work required to host and deploy this model is done! Awesome! 


**Querying the model**

Now we also want to query this model. I stated earlier that we wanted to see 12 months of forecasted cost numbers and this is how we do that.

<pre>
SELECT forecast_value 
FROM ML.FORECAST(MODEL my_arina_model, STRUCT(12 AS horizon))
</pre>

It is as simple as that.


# Use case 2: Next month cost forecasting

![jp.png](/uploads/jp.png)


In this use case we want to estimate the cost for the next month. We continuously track and save events that we know will affect our costs, but we do not know exactly how or when. 

Here we are no longer looking at a time series problem, although time is important, but more of a traditional ML setup. We need to create a dataset that we can train our model on with the cost for next month as the target variable, i.e. the cost we want to predict.

**Data**

So we have two datasets: one with the raw events and one with the monthly costs we looked at in the first use case. First we need to transform the raw events into features in some way. One simple way could be to count the number of events per month.

<pre>
Aug - 12 events
Sep - 5 events
Oct - 3 events
Nov - 8 events
</pre>

But since we know there is some delay between the events happening and the actual costs we need to “lag” the data. This means that for every row (with features and cost) we also include data from previous months. So the data would look like this:

<pre>
Month, Actual cost, Count of events previous month, Count of events 2 months back
Oct, 265806, 5, 12
Nov, 288615, 3, 5
</pre>


When we use this data to train a model we basically say:

*“I have these two values, 3 and 5, if you see them you should output 288615”*


**Training the model**

We now have a dataset that we can use to train a model. A suitable first model for this problem is linear regression which as we saw earlier is one of the models BigQuery ML supports and here is basically how you would do that.

<pre>
CREATE my_lr_model
   OPTIONS (model_type = 'LINEAR_REG' ) AS
   SELECT * FROM (
      SELECT totalAmount as label FROM (
           LAG(event_count, 1) ... as count_lag_one,
           LAG(event_count, 2) ... as count_lag_two,
		...
	)
</pre>

Relying on the default settings for the linear regression model the configuration for this is ridiculously easy. But you may need to tweak it and there are quite a few knobs to turn. Also note the handy LAG operator in the SQL which works just like “shift” in the Python Pandas’ world.

**Querying the model**

Now that we have a model we also want to use it naturally. All it takes is some SQL and here is one way to do it:

<pre>
WITH t as ( select 4 as count_lag_one, 8 as count_lag_two )
SELECT * FROM ML.PREDICT(MODEL my_lr_model, (select * from t) )
</pre>

I provide the incoming data in the WITH statement and then ask the model to do a prediction based on this data. We deploy and schedule the training of this model as in our first use case.

# Summary

BigQuery ML has certainly made working with AI and large datasets seductively easy. It is a natural first stop on your AI journey to quickly get one of the supported models up and running. Eventually it does however require a little planning of how you structure the data, so you can feed it into the models in a simple way. The next step in your project will probably also require you to take a step back and do a little EDA (Exploratory Data Analysis) outside of BigQuery to better understand your data. I recommend Google Colab. After that, there is a big chance you will just go back to BigQuery ML, but now armed with your increased data knowledge you will probably tweak the models a bit. And if you feel you have outgrown the supported models Google also provides many other ways you can run AI models. Happy BigQuery coding!


Torbjörn Stavenek
