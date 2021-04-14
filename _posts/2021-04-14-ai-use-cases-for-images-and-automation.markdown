---
title: AI use cases for images and automation
date: 2021-04-14 14:58:00 +02:00
---

In this article I will discuss AI solutions based on images, automation and the value of non-perfect models. I will include some important considerations for your AI projects, how to think and what to focus on. I will use a few different use cases to illustrate my points and hopefully this will get your inspirational juices flowing. 

# Effort vs correctness

One important thing to keep in mind when working with AI solutions is that the results or correctness do not necessarily need to be perfect. Sometimes good enough will do. There is almost always a trade off in the amount of time you have to spend on perfecting your model and the improvement you get. It is like when you try to accelerate to the speed of light: in the beginning it is quite easy to get up to a decent speed but the closer you get to the actual speed of light the required energy increases infinitely. So it is often a good idea to start out with the simplest thing you can imagine for your specific use case. 

Often the real value of AI is not in exactly how perfect your solution is but rather in the automation part of it. Say you try to decide if an image depicts a flawed or flawless product. From an AI usage perspective this is a simple classification - flawed or not. But if you look behind the curtains you actually get a number from the AI model that also says something about how sure the model is of its decision. 

Let us assume that 100 means flawed and 0 means flawless. In some sense the AI is counting defects. So if the AI says 0, well we can be pretty sure this product is ready to ship. But what if the AI says 10? It is still a long way from 100 which we consider a flawed product to be thrown away. So we should probably also ship this product. But where do we draw the line? In the implementation of this use case we have to pick a threshold value, normally this is 50. Below 50 means ok and above means flawed. If we were extremely picky about not shipping flawed products we could lower this threshold to catch even the slightest doubt about the quality. Let us use 50 as a threshold for now.

However, let us get back the previous discussion about correctness. If you have to look at one million images per day manually you will soon zoom out and start thinking about your cat back home. But if the AI would check your images for you and only forward those it was not very sure of, i.e. those close to the threshold, say with a value of 40 to 60, you would save yourself a lot of picture-looking.

In this case the AI would do the bulk of the work and look at all images. For those it was extremely sure about, flawless if  below 40, and flawed if above 60, the human ie you would not need to do anything. You would only be required to take a decision when it comes to the (hopefully) few remaining images the AI could not make a clear decision about. This is the real gain in this use case - automation. Even for an initial simple and non-perfect AI model this would probably save you tons of manual work. Then when you learn what the AI has problems with, you can start collecting those images and retrain your AI to become even better, further reducing your workload. Sounds nice right?!

This use case is so common that AWS even provides a specialized service for it: Lookout for Vision. 
Read more about here: http://praktisk.ai/posts/aws-lookout-for-vision-goes-ga/

# Looking at graphs
Just as in the previous example a human can also look at a graph of some vital measure and decide if there is a problem. But with 100sd or 1000s of graphs to go through this is not feasible to do in a timely fashion for a human workforce. An AI will do this for you, tirelessly and 24/7 and report any found potential anomalies for humans to take a closer look at. This is also an automation made possible by AI, but that also does not require a 100% perfect accuracy.

Also see smart buildings blog.
https://praktisk.ai/posts/smart-buildings-with-ai/


# Counting and other hobbies

Another interesting use case is when a single item you have to analyse as a human takes a very long time, but the AI can do it in a second, like counting objects. There are a couple of different techniques to do this. If you ever worked with object detection and object localization you could just extrapolate this: detect all objects of a certain type in an image and then count them. This works well for counting small numbers of objects, say below 100. 

<Images of faces, cars in parking lots, queues, >

But there is also another technique called “crowd counting” that we can use when we need to efficiently count 1000s of objects in a picture. This technique works by converting images to blurry density maps and training a model on those. It actually works surprisingly well!

Here is an example of counting people in an arena. As with all supervised learning you need a ground truth, so someone had to “dot” every person in the crowd (the red-dotted image on the left) so the AI has something to train on. This part of working with AI is not that exciting…


 



Here is another example involving bees. Look at the image and try to estimate how many bees you see! This is obviously a good use case for AI.






# Classification of potential skin cancer


This image displays a range of different types of skin cancers and perhaps non-cancer skin marks. I have no idea. But let a doctor classify the images and train an AI model and you (or the doctor) will all of a sudden be able to handle magnitudes more patients. Those that the AI classifies as non-cancer with high confidence you would never need to bother a doctor with. 

However, this is one of those cases where it is really hard to know what the threshold should be. Exactly how confident do we say the AI has to be for us to classify this is “clearly cancer” or “clearly nothing”? We carefully need to consider the consequences of telling the patent the wrong thing - good or bad! 
But then again, in developing countries for instance this type of solution could provide value because the alternative is “nothing” and so the value of catching obvious skin cancer cases would be high. 




# Summary

I discussed a number of different AI use cases revolving around images and how to use these kinds of solutions for automation. I hope you learned something to use in your own business! 




*Torbjörn Stavenek* - 
[LinkedIn Profile](https://www.linkedin.com/in/tstavenek/)