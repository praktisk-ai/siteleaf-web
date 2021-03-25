---
title: Anomaly detection overview
date: 2021-03-25 10:29:00 +01:00
image: "/uploads/bulbs.png"
---

Anomaly detection is the notion of automatically looking at data and finding outliers, i.e. data points that do not fit in well with the rest of the dataset. Finding an outlier is usually a sign that something exceptional has happened that should trigger further investigations or actions. Anomaly detection can be performed on existing historical datasets e.g. when you are pre-processing your AI training data. It can also be used on a real time data stream from your business transactions or sensor readings to almost instantly flag a data point as anomalous. This is a great example of AI automating things that a human can do and with a glance at a graph see a problem. Doing this manually for 100s or 1000s of datasets is not feasible though and this is where the relentlessly working AI can come in and do it for you. Simply looking at graphs is also something a human can do well for data in one, two or even three dimensions. With higher dimensional data things get very hard to visualize and this is also an area where an AI can shine. In this article I will discuss anomaly detection from a few different perspectives and with this new tool under your belt you will start to look at the world with different eyes!

# Forecasting based
Many use cases for anomaly detection has to do with time series data. Perhaps you have sensor readings coming in every hour and you would like to know as soon as possible when readings start to deviate from the expected range. For these use cases we can use a “forecasting based” approach.
This is an approach to anomaly detection that is very suitable for time series data since there exists a large number of tools designed for this. You basically do a normal time series forecasting of the data and measure the error compared to the actual value. You will then get a distribution of errors and the idea here is that if your specifically forecasted error exceeds some limit, say 3 standard deviations (99.7% of the errors will fall inside this range), then we will flag this data point as an anomaly. Another way could be to calculate a confidence interval based on the prediction and flag the actual data point when it falls outside of this interval. 

![ts-std.png](/uploads/ts-std.png)

In the image above the actual real data point (blue) is outside the predicted and tolerated confidence interval and thus flagged as an anomaly.

AS an example of a high level anomaly detection service take a look at “AWS Cloudwatch Anomaly detection”. All you have to do is get your metric data into Cloudwatch and “tick a box” to enable the anomaly detection. You will now be able to trigger alarms on anomalies and also display the  metrics with confidence intervals in the console. Below is what it looks like.

![aws-cw-ad.png](/uploads/aws-cw-ad.png)

# Regression based
Closely related to the forecasting based anomaly detection is the “regression based” detection that can be used more generally to look at data and predict a value. Assume you have a website where people sell stuff and they can set whatever price they want. You as the site owner could predict the price of their article and alert them to the fact that their price seems to be “too high” or “too low” based on historical sale prices for the same kind of article. 


# Outlier detection
When your data is not a time series and you have to resort to other methods, which can however also be applied to time series data if you like. This is just a matter of how you organize your training data. 

Look at the image below, it clearly has some outliers and a cluster where most of the data seems to be located.

![2d-anomaly.png](/uploads/2d-anomaly.png)

Applying two different outlier detection algorithms to this data gives us these two decision functions for some (not very good…) hyperparameter settings.

![anomaly-analysis.png](/uploads/anomaly-analysis.png)

The orange area is where the specific algorithm will classify a data point as “normal”. You can see that the learned decision functions for these two algorithms do not completely align. That is why ensembling several algorithms, i.e. combining several different types or algorithms, can sometimes be a good choice to get a better “averaged decision function”. 

The algorithms used here come from the very competent Python package PyOD. However, if you like to use algos better supported by say AWS they have something called a Random Cut Forest, which is a variation on IsolationForest in SciKit Learn.



# The real world
Anomaly detection is not a magic tool. You cannot just throw a bunch of data into the detector and hope it will find whatever is an anomaly in your specific domain. As with all AI solutions you need to know what you use as training data, how you define your features and what you are looking for. Based on this you can configure and run your model training and with this well defined scope you will have maximized your chances of success. 

Sometimes you are looking for anomalies in your data but it does not really fit into the previously described approaches using the raw or unprocessed data. This could be for instance some KPI you are monitoring that is based on some perhaps complex calculation from the raw data. This is what is called “feature engineering” and is very important when it comes to complex domains. Identifying these KPIs and providing the AI with a training dataset of this processed data will again take you back into the anomaly detection territory where the AI really can do magic.

#Use cases
There are an endless list of use cases for anomaly detection and here are some examples:

* Detecting fraudulent transactions in real time. This is something AmEx is doing very well with millions of transaction analyses per second
* Electricity consumption for buildings
* CPU usage
* Transaction volumes in business or IT systems
* Web page views
* Average order value
* Foot traffic into a building
* Temperature in room
* Vibrations from manufacturing machine
* Network traffic
* Recorded sound


# Summary 
Now you know a little more about anomaly detection, use cases and some pitfalls. As usual it all comes down to the use case: what are we looking for, what data can we use and who wants to know about the results. Happy anomaly hunting!


*Torbjörn Stavenek*
