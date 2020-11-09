---
title: Getting started with AI - part 2
date: 2020-11-09 10:49:00 +01:00
image: "/images/spiral.jpg"
---

Many companies have no idea about how to get started with AI in their business. Recently I even heard about a company which despite having the AI skills in-house, still did not know how to apply AI practically. So this is a common problem. 
This is the second post in my series about getting started with AI, and in this post I will continue to explore some general use cases that can be applied in many different kinds of industries. Here is a link to the first blog if you would like to read that first: [Part 1](https://praktisk.ai/posts/getting-started-with-ai/)


# Intro
Many simple and effective applications of AI are very general and not specific to any particular industry. This means they can be applied in many different companies in many different industries which make them even more robust and useful. This general applicability also drives service providers to implement generalized applications and services which reduces costs for all parties.

In this post I will discuss the following use cases:

* Customer segmentation
* Location optimization
* Product recommendations


# Use case: Customer segmentation

![clusters.png](/uploads/clusters.png)

Often it can be advantageous to divide your customers into groups. On the scale from nothing to full-on one-to-one personalization, segmentation is somewhere in between. Even though segmentation can be a good thing for companies with only a few customers, to really benefit your company probably has a very large number of customers. In AI this is called clustering and is used to create groups of similar customers. Here are some simple example metrics you might manually group customers according to:

* Most active last day/week/month
* Most money spent last day/week/month
* Income
* Job
* House vs apartment
* Address
* Bought product or service X
* Number of employees 
* Revenue and profit

However, the really interesting groups are usually discovered by the AI when it is given a bunch of data about each customer and then has to figure out the groups itself! This is when you can unravel completely new insights about your customer base! 

So now you have a number of customer segments. What can you do with it? Well, you probably want to contact them about something like:
* New product or service offerings
* Discounts
* Upselling/Cross-selling offerings
* Special event invitations

I know of companies that refer to some segments as “VIP” and flag them in the call-center support system to make sure they get the best possible support experience.


### Why do this
All customers are not created equal. Some are more valuable to your business than others - these are the ones you want to keep by making sure they love you. The customer segment at the other end, well, you don’t want to keep sending discounts and rebates their way, do you?

### How do I get started
To find the most interesting segments of customers you need to collect all possible data about them available to you. This could be data from your CRM (Customer Relationship Management) system, your website, your email marketing system, information about the customer location, etc. Of course you can start small with the data you can easily get your hands on, but the more the better. Then you let the AI do its magic and if you are lucky you should be able to reveal some new interesting things about your customers.


# Use case: Location optimization

![location.jpeg](/uploads/location.jpeg)

Does your company from time to time need to find the optimal geographical location for something? Perhaps you need to find the best spot for a new store, storage facility, windmill or food truck? To solve this we can use either of two different methods. But first we need to collect a number of location examples. The more examples we have the better the AI will be able to understand what we want it to learn. For each location we need to add as much data as we can find, like:

* Average outside foot traffic 
* Proximity to highway
* Windy days per year
* Number of people working in the area
* Day of week

If we treat this as a “classification” problem we also need to label every location as “good” or “bad”. We then train the AI on this data. Now we can show the AI new locations and it will tell us (classify) if it considers a particular location to be “good” or “bad”. Pretty neat.

Sometimes the good vs bad classification is not fine grained enough. Then we can treat this as a “regression” problem instead (meaning the AI will predict a number for us). To do this we need to assign a number instead of a label to each location. So a top location could be a 10, a bad location 1, and others somewhere in the middle. We train the AI and it will now be able to give us a number for new locations we show it. This way we are able to rank a list of possible new locations. Even neater!

### Why do this
If you need to choose between several possible locations, why would you not want to pick the best one? Of course you want the one you hope will be attracting the most foot traffic, take the least time to drive a truck to, or generate the most electricity. As in many use cases us humans normally have a hunch, but there are too many interacting variables to know for sure. This is where the AI can shine and help us out with its opinion about something based on the examples we have trained it on. So this is a perfect way to make sure your company maximizes its revenue!

### How do I get started
You probably have a list of potential locations you are choosing from as well as the example data you want to base your decision on. You need to enrich each item on this combined list with the data you want the AI to consider. This could mean that you have to dig into external data sources, maybe you have to buy statistical data or weather reports, or even just perform lots of web searches. You can even add images of the neighbourhood as input data about a location! Remember, the more relevant data you have, the better the AI’s predictions. 

For a retail store chain, here are some example data points to collect:

* Distance to nearest city center
* Distance to nearest highway
* Number of people living within 5 km from location 
* Store size
* List of all departments in the store

Now when you train an AI with this data perhaps it will find the relationship that “big store, close to highway and has many departments” will be rated as 8. But perhaps it will also find that “small store, close to city center with many people living nearby” is a 10. 

For a food truck here are some example data points to collect:
* Street 
* People working in the area
* People living in the area
* Day of week 
* Day of month
* Season
* Today's special was X

This might reveal that “on tuesdays in the summer” going to Street A will always be your best choice.

Remember that you yourself have rated the examples, so the AI will put a higher valuation on the things you have shown it to be more valuable. Hopefully this gives you a better material on which to base  your final decision.



# Use case: Product recommendations

![recommendations.jpg](/uploads/recommendations.jpg)

Product recommendations is something most people have seen. Some of the most known examples are Netflix movie recommendations and Amazon product recommendations. Entire companies have this as their core offering - helping other companies do it better. But with a little help from AI, doing this well has now become much easier than before. Many companies also have or are planning to incorporate this into their marketing or selling strategies. And for obvious reasons. Showing attractive additional products or packages is often a good way to increase sales. 

Traditionally this has been a purely statistical or analytical operation: “on the web page for product X, recommend things that product X buyers also bought”. This is a nice and simple way to start if you have never done recommendations before. But if you wanted to apply AI to this problem - could you? The not so surprising answer is “you could and you should”! As always you want to collect lots of examples to train your AI including things like this:

* Current product name (on the current web page)
* Current product price
* Visitor total time on website
* Pages visited before current page
* Geographical location of visitor
* Total visitor purchase history

With this information the AI will be able to learn the best additional product suggestions for each main product. 

### Why do this
When your customers have found something that they want, why not help them to find valuable and useful extra products? This could be a great service for your customers that set you apart from your competition. Of course this is by no means an act of altruism - since you will sell more and make more money. So it is a win-win situation! 

### How do I get started
A good way to get started with this is to pick out some high-volume products. Because the volumes are high you can set up A/B-testing which means that for some customers you show recommendations and for the others you don’t. You now have a way to compare the impact of the recommendations. You also limit the effort required to collect data and train the AI, and you will only affect a part of your business (“small blast radius”). Due to the law of large numbers you can have higher confidence in the results than if your product sells only a small number of items, since as the number of purchases grows the observed effect gets closer and closer to the “truth”.

This could be a rather big project depending on your internal-IT and general IT-maturity. But if you just want to “get it out there” there is a shortcut.

The fast way to get started is just a few steps away:
* Find the data
* Train and deploy your model
* Insert some code in your product webpage
* Done.

This is such a standardized service that almost all major cloud providers offers this via API integrations, here are a few:

* AWS Personalize
* Google Recommendations AI


# Summary
This is the second article in this series where I discuss general AI use cases that are applicable in many industries. I hope you learned something and are now eager to get started with AI in your own company!

Torbjörn Stavenek 
