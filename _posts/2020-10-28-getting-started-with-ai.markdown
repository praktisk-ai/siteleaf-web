---
title: Getting started with AI
date: 2020-10-28 14:52:00 +01:00
image: "/uploads/female.jpg"
featured: true
hidden: true
---

Many companies know about AI and also know that they should start looking into it. However, many companies do not know where to start. Many companies also doubt the value of AI and consider it to be fuzzy and unclear -  and for good reasons! There is much hype and hand-waving when it comes to the actual practical uses of AI. Many companies even claim to use AI when in fact they are not. This of course does not help.

In this article I will look at a few general AI use cases that are applicable across industries to get your own ideas flowing. I will not assume that you know much about AI (artificial intelligence) or its subfield ML (machine learning).

The intended audience is people who want to know how AI can help their business and how to start on their AI journey. So let’s get started!


# What is AI really

Here is a simple definition just to give you an idea of what AI/ML is:

*Artificial intelligence is the science and engineering of making computers behave in ways that, until recently, we thought required human intelligence.
Andrew Moore, Carnegie Mellon University*

So in simple terms it is a way for computers and algorithms to automate tasks that require a small amount of human thinking, or even tasks that are impossible for humans.

An example of a small thinking task could be looking at the text of a product or service review and categorizing it as positive or negative.

An example of a task impossible for a human could be forecasting product demand based on hundreds of variables like: expected number of visitors, weather predictions, day of week, season, road traffic patterns, location, stock market movements, etc.



# General AI use cases

Here are some general use cases, applicable in many industries, that I will discuss below:

* Sales forecasting 
* Customer churn 
* Review sentiment analysis


# Use case: Sales forecasting 

![forecast.png](/uploads/forecast.png)

Sales forecasting is a way of looking into the future and predicting for example how much money your company will make during the next month. This can of course be applied to any KPI, at any part of the organization and for many different forecasting periods. Some examples:

* Total company revenue for the next six months
* Profit tomorrow for retail store number 23 
* Number of sales transactions across locations in city A
* Demand for product X every day during the next 7 days


### Why do this

Think about what you would do differently in your day to day business if you could peek into the future and know some of the numbers in the examples above. Is everything going as planned? Why does the AI claim that my profit will drop next month? Should I get more staff next week to meet a visitor surge? Should I stock up on a certain product segment to meet a predicted increased demand?

Optimal stock management is probably the holy grail of business optimization for many companies. Inventory ties up capital so we want it to be small. But not so small that we lose business because of unfulfilled orders. It is a fine line! Demand forecasting is the first step towards optimizing your inventory.

So doing this could save you money if you get early warnings about decreasing business to eg replan strategy or staffing. It could also make you money if you avoid running out of in-demand products thus losing sales.


### How do I get started

To get started you need data to train an AI model. This type of problem is called “time series forecasting”, because you need a series of numbers collected over a period of time. Say you want to predict “sales for the whole company next month”, then you should find this same kind of data to train your AI model. 

Sales data is normally available in the business system used by a company. Or perhaps your IT-department already has started collecting data into a Data lake? Either way, find someone with the proper access and they can normally export the series of numbers for you to Excel. Next step is to give the Excel to a data scientist for analysis to find out if it has any predictability. If the data actually has signs of predictability then it is time to run a POC (proof of concept) project to get the real live predictions into the hands of people who care about them. 


### Example

For the prediction case “sales for the whole company next month”, you should find this data to train your AI model. Here is a short example time series:

* January 12
* February 15
* March 8
* April 9

During training the AI would look at this data and try to create a model to predict the next value in this series of numbers. Maybe it would predict 10 for May. Good! You now probably know more than before. Maybe it was exactly what you yourself predicted? Also good! The AI agrees with you. 

Come end of May, you collect a new number from your actual total company sales the past month, let’s say it was 8, and send your time series into the AI model so it can forecast a number for the coming month. The AI predicts 5. What?! According to your own calculations your sales should be 10!  Why does the AI predict only 5? The AI may be wrong of course, but it may also have spotted something you missed. But at least now you have been alerted to this potentially disastrous next month's sales, and can look into it to find out if this is in fact what will happen and if so do something about it!


# Use case: Predict customer churn

![churn.jpeg](/uploads/churn.jpeg)

Losing an existing customer is bad for business. Your stream of income gets interrupted and you have to devote resources into finding new ones. So preventing customer churn is good. But how can you do that? Well, there are sometimes a sequence of tiny things that indicate what is about to happen: less sales, smaller sales amounts, less reactions to marketing, less service usage, fewer services used, changed usage patterns, etc. Couple this with all available static data (name, address, industry, age, …)  on the customer and you can start answering questions like:

Will this particular customer churn next month?
How long before this particular customer will churn?
What is the total lost sales value due to customer churn next month?
How many customers will churn next month?



### Why do this

How does your company handle customer churn after the fact? The customer has already changed his mind, taken action to leave and moved on. Getting this customer back is really hard. Now, if a customer is about to leave but has not taken any actions yet, ie he is still a customer, that is a much more preferable situation. The customer is not lost and so much easier to “win” back! Would you not be interested in knowing this about your customers before they leave?

Doing this would make you money because an existing customer will continue to buy from you. It will also save you money since you will not lose that existing income, and you do not have to spend money on finding replacement business.


### How do I get started

The CRM (customer relationship management) system in your company normally holds data about your customer, both static data and data about all interactions. In smaller companies maybe the CRM is an Excel or some other rudimentary customer record. In this case there is probably only the static data available here - which is not enough. You also need interaction data, which you can get from POS (point of sales), website logs, phone records, sales database, etc. 

The important thing about the data is that you need examples of both types of customers: existing active customers, and churned inactive customers. This way when you train an AI model it can learn what separates churned customers from the still active ones, it can follow the history of customer interactions and find the early warning signs of churning customers. 


# Use case: Review sentiment analysis

![sentiment.jpeg](/uploads/sentiment.jpeg)

All customers have opinions about your products or services. Some of your customers will write reviews about your products, sometimes where you can find them and sometimes in hard-to-find places. Wherever written it is always good to have a sense of how your customers feel about you and your products. If you have many products this can be a very big and tedious job, perhaps impossible. All you really want is one or a few numbers like these examples:

* 96% of the reviews where positive
* 5%  negative, 75% neutral, 20% positive
* 81% positive, previous month 90% positive


Automation and AI is the answer! Automate searching and collecting reviews and let an AI perform sentiment analysis on them.


### Why do this

It is obviously good to know how your customers feel about you, but keeping track of this could be a factor for your company survival. So while you will not make any money doing this, getting early warnings about customer dissatisfaction could save you tons of money in the long run.

### How do I get started

Your first step here is finding a location online with reviews about your products. Once you know where they are you can start a POC project that will collect reviews, perform a one-off sentiment analysis of them and present you with the result. If the POC is successful you will probably want to take the next step and make this something that runs continuously. After this you can add more locations to collect reviews from, or even get a search tool to find them for you. 


# Summary

In this article is discussed a few general AI use cases and the first steps to get started with them. I hope you now have some ideas about what to do next in your company and how to get started on the AI journey. I plan to write more articles about both general and industry specific AI use cases so stay tuned. 


Torbjörn Stavenek


