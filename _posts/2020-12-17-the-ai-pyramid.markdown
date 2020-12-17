---
title: The AI Pyramid
date: 2020-12-17 10:29:00 +01:00
image: "/uploads/pyramid.jpg"
---

In almost all my conversations around AI I try to stress the importance of starting any new project with an MVP, a Minimum Viable Product. In the AI world, just as in the “real” world, this means starting with a use case. What is the benefit of doing this project and for whom? When you know exactly the use case or process you want to support then you also know what AI capability you need. In an MVP you don’t need the best-of-the-best AI solution, you only need a good-enough solution. You only need enough to finish your MVP quickly and prove that your use case provides benefits. Then during the evaluation of your MVP you can decide if and what parts need to improve during your next project iteration. Perhaps it is even the case that your initial “temporary” AI solution actually performs good enough to keep it!

So where do you find a good-enough AI solution? Let me tell you a little story about the AI pyramid. The AI pyramid (or AI stack) comes in all shapes and sizes. It is usually tailored to the services and products sold by the company describing it. So this means when AWS talks about AI their pyramid will look a little different and have a different layering compared to Google Cloud or Azure. However, I think a really nice way to structure the AI pyramid can be based on what you need to bring to the table (or pyramid…)

![ai-pyramid.jpg](/uploads/ai-pyramid.jpg)

# Top layer: Bring nothing

This is the easiest and fastest way to get started with AI. You bring nothing because your use case involves only generic data that anyone can collect and create a generic service from it. 

If for example your MVP needs to count people in a picture then this is not a very unique requirement. To build it yourself you would have to collect tons of pictures with people in them, train an AI to find those people and deploy the model. But this is your lucky day! Because this is such a generic use case, a service to find people in a picture is just an API call away, like these:

* [Azure: Cognitive Services - Computer vision ](https://azure.microsoft.com/sv-se/services/cognitive-services/computer-vision/)
* [Google Cloud: Cloud Vision API](https://cloud.google.com/vision) 
* [AWS: Rekognition](https://aws.amazon.com/rekognition)


In all of these services someone else has already done the job for you. Those big companies have the resources to create really good generic services and you should leverage that if you can. Perhaps they are not exactly what you need or they are not as accurate as you had hoped for. But as I stated before: those services are just an API call away, so if you can - start with here, and especially now that we are talking about an MVP.

So anytime you need some AI you should always start looking for ready-made services at the top of the pyramid.


# Middle layer: Bring data

Maybe there is no generic service available for your specific use-case, or perhaps you are not happy with the accuracy of those services? This is a case where you have to bring your own data. 

This could for example be that your use case requires you to find “people with funny hats”, but the generic services can only find you “people with hats”, or even just “people” and “hats”. In this case you have to provide your own customized training data. You have to do all the work of collecting pictures of people with funny hats and then in every picture you have to mark every single one of those people.

Luckily many companies also provide services to help you with the work at this level. You may have heard about the concept of AutoML, and this is exactly what that is: you provide the data and someone else handles all the rest. A bit more work for you, but still much less than in the bottom layer. Some of the available services you could use in this middle layer to help you out with the hat-people example:

* [Google Cloud: AutoML Vision](https://cloud.google.com/automl)
* [AWS: Rekognition Custom labels](https://aws.amazon.com/rekognition/custom-labels-features/)
* [Azure: Automated ML](https://azure.microsoft.com/en-us/services/machine-learning/automatedml/)
* [Peltarion: AI Platform](https://peltarion.com/platform)

A fun way to play around with this is “Teachable machine” where you create simple data in the browser and then get a working model in seconds.


[Teachable machine](https://teachablemachine.withgoogle.com/)
![tm.png](/uploads/tm.png)


# Bottom layer: Bring code and data

In this layer we basically have to do everything ourselves: manage data, code, models, operations - all of it. But you can still benefit a lot from services provided by the cloud providers. 

**EDA**

When doing exploratory data analysis (EDA) with Jupyter notebooks you can run it all on your own laptop. But why should you when there are several free and paid services out there, most even provide GPUs so you don’t have to bother with the cumbersome GPU driver installations. Some examples:

* Google: Colab, https://colab.research.google.com
* Kaggle: Kernel notebooks, https://www.kaggle.com/notebooks
* AWS: SageMaker notebooks, https://aws.amazon.com/sagemaker/

**Storage**

Then you also need to gather and manage your data and datasets. Storage solutions are easy to find so I will not list any here. Just remember that collecting data for the MVP is probably a quite easy one-off thing, compared to the continuous process of getting the new data that you need to keep your deployed AI models up to date.

**Labeling**

You need labeling to actually draw the boxes around your hat-people, and perhaps do some data augmentations to get more training data, and here are some nice services:

* Labelbox, https://app.labelbox.com/
* RoboFlow: https://roboflow.com/features
* AWS: SageMaker Ground truth, https://aws.amazon.com/sagemaker/groundtruth/
* Hasty, https://hasty.ai/

**Training**

With your labeled data and your code ready it is time to do the training. For an MVP you would probably just run the training in your Jupyter notebook, but later on you will need to set up continuous training. Training could be something small and quick that fits within a serverless function (nice!), something huge requiring several days of GPU computations distributed over many machines, or something in the middle that you can execute as a Docker container on a cluster somewhere. The major cloud providers have you covered! Your options:

* Serverless functions
* Containers
* Virtual machines
* Cluster management services

**And more...**

You need to deploy your new fancy model somewhere. You need to monitor it so the service is available. You need to look out for model drift so the model delivers correct predictions over time. And you will eventually need a data and training pipeline to do all the data collection and data management and model retraining on a regular basis.

**Platforms**

As you can see, down here in the bottom layer you have to do an awful lot of stuff yourself. If you find yourself down here, have a look around at some of the all-in-one offerings out there before rolling your own. They do much for you out of the box, although it can be a bit non-flexible. Also these platforms can get quite expensive for small startups. Here are some AI platforms to consider:

* AWS: Sagemaker, https://aws.amazon.com/sagemaker/
* Google: AI Platform, https://cloud.google.com/ai-platform
* Peltarion: AI Platform, https://peltarion.com/platform



# Summary

In this article I presented a generalized AI layering model, the AI pyramid, based on what you yourself need to do in each layer. For illustration purposes I used a random use case of finding people with funny hats in pictures. For other use cases there are of course many other ready-made AI services you could use. As you can see we have to do more and more work the further we get from the top of the pyramid. So obviously when we need AI, we should always start at the top. For each layer of the pyramid we should carefully consider if we can use what is available before moving down to the next layer. So good luck with your next project and hopefully you can stay at the top!


Torbjörn Stavenek

