---
title: Smart buildings with AI
date: 2021-02-18 09:49:00 +01:00
image: "/uploads/city.png"
---

Buildings and facility management have a large number of AI use cases. Most of them are about minimizing cost and waste, some are about optimizing energy and usage efficiency, and a few are about improving the lives of the visitors or inhabitants. Using sensors of some kind is often a prerequisite to be able to gather the data used by the AI. In this article I will discuss 7 valuable AI use cases you can apply in your facility management business.

Use cases
* Anomaly detection
* Visitor forecasting
* Active energy optimization
* Predictive maintenance
* Gap filling
* Elevator usage
* Chatbot

**Anomaly detection**

![leak2.png](/uploads/leak2.png)

Anomaly detection is an area which has many applications and I will walk you through  a few of them below. 

Energy consumption is often a large cost when it comes to buildings. There is a cost for both heating and cooling, and ventilation, and all combinations... This is what makes it so interesting to try and optimize - even a small improvement can translate into a lot of money. So if you have a single graph of energy usage for a specific room or building, it is pretty easy for a trained eye to spot any irregularities. You immediately see when things started to deviate from the expected. You have found an anomaly. Then of course there may be other factors to consider like outdoor temperature or huge number of people, etc. But apart from this, it is pretty straight forward. This is not the problem. 

The problem is when you have 100s or 1000s of graphs with sensor data. It is just not possible to manually monitor them all. Either you don’t have the staff needed, or if you do it is not a very exciting job. AI to the rescue! This is the perfect thing for an AI to do: learn patterns to look for, and then tirelessly perform this boring task day and night for as many graphs as you have available. Problems will also be detected faster and can be acted upon before escalating. This also frees up you human staff to do “real” jobs, like actually looking into and fixing the problems discovered.

This use case with an energy viewpoint can help you find problems with radiators, broken or open windows, etc. Anything that would make the energy consumption go up or down. This could then trigger an alert to the human staff to look into this anomaly.

This use case can also be applied to water. If you have sensor data about water consumption this can let you find leakages, broken or non-closed toilets and basins, etc. Water may not be as expensive but the savings are not only monetary, but also helps you identify failed equipment before any inhabitant gets angry enough to report it. So this improves the quality of your buildings and the happiness of your inhabitants!

There are also other air quality sensors that can be monitored for anomalies like radon and CO2 and which can also be made to trigger alerts when they fall outside normal values.

**Visitor forecasting**

![crowd.png](/uploads/crowd.png)

If you could have an AI learn and forecast the number of visitors to your building, what would you change? This can impact many things when it comes to facility management: staffing, purchasing, energy consumption, etc. Or if you run a commercial building, forwarding this information to your tenants provides added value, makes them happier and allows them to optimize their business. Depending on how fine-grained sensor data you have available, you as a landlord may even be able to foresee tenants with problems ahead whose foot traffic is declining, and take actions to help them. 

So if you expect a lot of visitors you can make sure to staff up properly, and vice versa. A restaurant expecting fewer customers might consider buying less of the expensive perishable stuff they have on the menu. And an office may decide to close down some parts to save energy when it seems very few employees will be there.


**Active energy optimization**

We come back to energy again, this time looking at it from an operational point of view, how to actively and continuously tune the energy usage. The AI can be taught what needs to be done to operate a building in a desired way. By why just have the AI tell you to “turn this knob to 5, and this one to 3” - why not let the AI do this by itself?! This has been implemented by some innovative companies like [DeepMind and Google ](https://deepmind.com/blog/article/deepmind-ai-reduces-google-data-centre-cooling-bill-40)in a few of their data centers, claiming to save up to 40% in energy cost! 

In this scenario you tell the AI what you want to achieve, like the acceptable range of temperature, humidity, CO2, etc. Then you tell it what “levers” or “knobs” it can use to achieve this. You start it up, cross your fingers and you wait. This is an area within AI called reinforcement learning, where the AI observes its environment and what happens as it manipulates it by using actions. In this case the actions would be “pulling levers” and “turning knobs”. As it explores the effects of its actions it slowly learns to control its environment.  
You would probably want to supervise the AI during the initial training...


**Predictive maintenance**

If you have sensors on things that can break - good for you! That means you can train an AI what constitutes normal sensor values and this means it can help you detect anomalies. Just as with the energy usage, detecting anomalous values can help us understand when something is not operating normally. Detecting problems early can trigger alerts to send human staff to investigate and fix things faster or preemptively, increasing overall quality and happiness.

This is useful for doors of all kinds, especially automated ones, elevators, escalators, etc. Sensors can detect that elevators stop a few millimeters above or below the floor, or doors vibrating more than usually when opening, or escalators change speed.
Moving things tend to wear down over time and eventually break. Collecting sensor data helps to identify problems early on and can also be used to calculate something called “Remaining Useful Life” (RUL), remaining time before failure. Imagine getting a list of building inventory ordered by RUL? Basically an itinerary for your staffs’ manual inspections!

**Gap filling**

Another interesting case is when you have sensor data and for some reason you get a gap in the data. This could be due to sensor failure, power outage, communication failure, etc. Anyway you have a gap in your data. Is this a problem? If this data is the basis for invoicing your tenants, then yes. In this case the AI could learn the normal patterns of the sensor data and do normal forecasting based on historical values. But since it is a gap we also know about the data coming after the gap so we can apply backward forecasting based on that “future” data and so we get twice the confidence in the forecasted gap. Now we can invoice with confidence!

**Elevator usage**

Some buildings have a lot of people coming or going during a few hours every day. When they all depend on elevators there is bound to be waiting involved, and no one likes to wait. What if the elevators could anticipate the floor where people would next want to “come aboard” from? Well, this has been tried with positive results. The AI monitors and learns the traffic patterns and then moves around autonomously when not in use trying to anticipate where the next person will come from. For large elevator systems this could also include elevators communicating between themselves to optimize the traffic flow even further.


**Chatbot**

Chatbots can be a useful tool for many things in the areas of providing information and collecting information.

Sometimes an FAQ on your webpage is not enough or your visitor cannot be bothered to go there and then find the most suitable answer to their question. A chatbot can greet them on the frontpage and offer immediate help or directions.

Giving visitors or inhabitants of your buildings an easy way to report problems is an essential part of providing a quality service as a facility operator. You empower people to help out, more eyes help you detect problems and the sooner you know about them the sooner you can fix them. Chatbots can be one way to add value to your service and can also help you collect better problem reports by guiding the visitors through the reporting process. 

**Summary**

I have discussed 7 different use cases for applying AI to buildings and facility management. Many of them provide cost savings, but some also improve the quality and service you provide to your visitors or tenants. I hope you will find that once you start on this AI journey there is no going back and  hopefully you got some ideas you now want to go and try out! Good luck! 


*Torbjörn Stavenek*
