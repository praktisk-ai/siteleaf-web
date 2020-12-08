---
title: AWS Monitron (re:Invent 2020)
date: 2020-12-08 08:41:00 +01:00
---

One pretty exciting new service from AWS is something called AWS Monitron. This is an end-to-end system for industrial equipment predictive maintenance. A bit like “ML in a box”. With predictive maintenance the goal is to get alerts when your manufacturing equipment starts to behave “strange”, which is usually a sign that it is about to break down. This gives you time to prevent or replace it before it even happens.

The reason why Monitron is exciting is because unlike many other AWS services, Monitron solves a problem end-to-end with very little effort. I always argue that you should use as high level services as you possibly can to bring down or eliminate development and maintenance costs. Monitron is as good as it gets: you buy the sensors, you download the app, glue your sensors to a machine, connect the sensors to your app - done!

You now have a complete end-to-end system with your sensors registering temperatures and vibrations, performing basic on-sensor raw data processing and sending the relevant stuff to the cloud. When the data gets to the cloud predefined ML-models will learn what normal behavior of your machine looks like for a few days. After that whenever something anomalous happens you will get alerted. You can also see graphs of the collected data inside your app. 

Another impressive thing is that you can do the whole setup in about 10 minutes. The cost for a starter pack with 5 sensors is around $700 which is small enough for companies of all sizes to get started. 


Read more here:

[https://aws.amazon.com/monitron/](https://aws.amazon.com/monitron/)
[https://aws.amazon.com/blogs/aws/amazon-monitron-a-simple-cost-effective-service-enabling-predictive-maintenance/](https://aws.amazon.com/blogs/aws/amazon-monitron-a-simple-cost-effective-service-enabling-predictive-maintenance/)

Torbjörn Stavenek

