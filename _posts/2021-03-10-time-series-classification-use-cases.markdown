---
title: Time series classification use cases
date: 2021-03-10 13:48:00 +01:00
image: "/uploads/crops2.png"
---

A time series is an ordered sequence of numbers over time. For certain problems you want to  look at a specific time series and from its characteristics be able to say something clever about it. Perhaps its origin or about some feature of the source. This is a class of problems called time series classification that I will discuss in this article, and when you start looking for them they pop up absolutely everywhere! 


# Intro
So, an ordered sequence of numbers can be created from many things. The source of this data may not actually be numbers but something completely different, like audio and speech, text, video, etc. However we can normally translate this “something else” into numbers. Let us look at text as a simple example. You would have to translate each word of a text to a number before it becomes a time series. But for lots of sources the raw data is actually numbers that may or may not require additional processing and some examples are sensor readings, monthly sales, temperature, etc. 

Now you have a time series in front of you. You look at it and think:

> “Hmm, that looks very much like the vibration time series of a cog that is not working!”

This of course happens to all of us all the time… Nah, not many of us could say something like this or would even ever be exposed to this situation. But in case it would happen here is what we should be saying:

> “Hmm, if only I had some time series examples of normal and malfunctioning cog vibrations!” 

In this case we could train a model to do the time series classification for us based on the examples we have collected. This is just one simple example and here are some other applications of time series classification:

* Who said what in a sound recording
* Activity type classification based on mobile phone movement
* Normal or malfunctioning equipment, based on temperature/vibration/sound sensor data
* Satellite image crop type 
* Identify individual bird’s in a forest
* Detect chainsaw sound in rainforest
* Classify EEG or EKG as normal or abnormal

It is also important to note that **time series classification** is completely separated from **time series forecasting**. In classification we are interested in labeling an already observed time series, whereas in forecasting we want to say something about the future.

# How do you do it
Like other types of supervised machine learning you need labeled training data. In this case you need a bunch of time series, each one correctly labeled. You then use this data to train a model that tries to learn what characteristics makes a time series belong to a specific class. The characteristics the model learns are patterns in the data that separate classes from each other. These patterns can be simple or complex depending on the specific problem and how similar the time series are. From a naïve user perspective we don’t need or want to know exactly what the model has learned. All we need to do is verify that it behaves as expected using our validation and test datasets.

# Use case: Who said what

![meeting.jpg](/uploads/meeting.jpg)

Transcribing, i.e. creating texts from audio recordings of speech, has been around for a while. It is used by tv-shows, pod-casts and support phone calls to get text based records of what was said. But what if you also want to know who spoke and not just an anonymous wall of text? Well, first you have to identify the different speakers and then for every spoken word or sentence you extract the sound data, convert it to a time series and finally classify this as belonging to speaker X or Y.

# Use case: Activity classification

![running.jpeg](/uploads/running.jpeg)

Almost all modern mobile phones have some health related apps available and they usually track different basic activities like walking or running and for how long you have been doing it. This is also a great example of time series classification. Mobile phones usually contain gyroscopes that deliver sensor data, and how about that?! A time series! Anyway, this time series is then classified as e.g. walking and the time you spend on this activity is recorded in your phone. 

# Use case: Crop type classification

![crop-type.png](/uploads/crop-type.png)

One interesting case of time series classification is based on satellite images. Some industries need to know how much of a specific produce is grown in a country and what the harvest will look like this year. This is something that can be done based on satellite images. For each pixel of the image the colour, mostly the green component, will be recorded over time to yield a time series. This time series can then be trained to classify the pixel as growing e.g. rice or sugarcane. Doing this over time, for all the pixels of all the satellite images for an entire country can give you a lot of valuable information. Be warned though that the computing power required is insane!

# Use case: Detect chainsaw sound

![chainsaw.jpg](/uploads/chainsaw.jpg)

One particularly cool use case is this: a group of people wanted to be alerted when illegal chopping down of rainforest trees happened. So they set up microphones at different places in the rainforest. Then they analysed this data continuously and as soon as the recorded sounds were classified as chainsaws they could send the police there to stop the criminals. AI to the rescue!


# Summary

Ok, hopefully you now know a bit more about what can be done with time series classification and what to look for.  I also hope this will help you identify cases within your own organization. Happy use case hunting!

Torbjörn Stavenek
