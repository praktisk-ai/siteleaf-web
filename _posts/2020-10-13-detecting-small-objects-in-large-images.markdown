---
title: Detecting small objects in large images
date: 2020-10-13 15:18:00 +02:00
image: "/uploads/little-dog-and-big-dog.jpg"
---

In this post I write about a use case where we wanted to detect small objects in very large images. I discuss the various abstraction layers in the AI-stack and when each is appropriate to use. For each AI-layer I also list some of the available options and commercial offerings, all from the perspective of the selected use case.

# Background
In one of our projects we get source data as large PDF-files. The objective is to locate a number of different, relatively small objects in the pages of this PDF. How can we do that? 

As always we start by enumerating a bunch of possible solutions, always going from the quick and easy all the way down to the manually implemented stuff.  The complete solution implemented as a proprietary piece of software we really want to avoid as much as possible. Why? Because although it can be tailored to your exact problem it also comes with a cost, the cost of operations and maintenance.


# The layers of the AI-stack
So what options do we have when it comes to finding these small objects? First of all it is a standard object detection problem. This is a known and well-researched problem. So there is a good chance we should be able to find a simple high-level solution. Here is our list of things to look into which also happens to be the layers of the AI-stack:


**Object detection as a service**

The first thing we should look into is if any of the big players (AWS, GCP, Azure) has anything useful offered as a service. Here are some options:

* AWS Rekognition
* Google Vision API 
* Azure Computer Vision API

In this case we need to locate custom objects, not just cats, dogs or people with glasses. So the most high-level services we cannot use, because they are trained to find only cats, dogs and people with glasses. Basically.

**AutoML**

The next level to look into is the AutoML offerings. Here are the options:

* AWS Rekognition Custom Labels
* Google AutoML Vision Object Detection

AWS Rekognition Custom Labels is a service where you provide your own images with your own annotations and AWS trains a model for you. In our case this could be a great way to get something up and running quite quick. Beware that deploying a trained model is rather expensive though.

Google’s version of this is called AutoML Vision Object Detection and basically offers the same thing.

Although this is called AutoML there is a lot of data pre-processing required to be able to use this for our case. We need first to convert the PDF to an image, then annotate it, and finally prepare the data in a format, size and structure that the AutoML product requires. As I also found out using these tools - the training is very slow. When you finally get the results there might be something you missed and you have to start over again. This makes the turnaround time very slow and you better have some other project to work on while you wait for the training to finish.

**The simple frameworks**

But wait, didn’t I just say this is a known and well researched problem? Well, yes… So we should be able to find some simple framework to try it out in “5 lines of code” ? We should. But in reality there exists only a few **that** simple-to-use frameworks. One of them is Detecto.

![detecto.gif](/uploads/detecto.gif)

[Model trained with Detecto](https://medium.com/pytorch/detecto-build-and-train-object-detection-models-with-pytorch-5f31b68a8109)


This is the “5 lines of code” complexity I expect of a good framework in this “mature” ML area. It is built on top of PyTorch and for simple use cases this is perfect. It also lets you go deeper when needed. You yourself control how much you want to train it and see progress. We get fast turnaround so we can tinker and test and iterate! Cool.


**The complex frameworks**

When you need to go even deeper and really get to State of The Art models here are some bleeding edge models to try out. Be warned though that at this level of the AI-stack you better know what you are doing, love to read research papers and handle library version conflicts. 

* [AWS Object Detection Estimator ](https://github.com/aws/amazon-sagemaker-examples/blob/master/introduction_to_amazon_algorithms/object_detection_pascalvoc_coco/object_detection_image_json_format.ipynb)

* [Facebook Detectron2](https://github.com/facebookresearch/detectron2)

* [Facebook Detr](https://github.com/facebookresearch/detr)

* [MMDetection](https://github.com/open-mmlab/mmdetection)

* [SimpleDet](https://github.com/tusimple/simpledet)



**From the ground up**

No one should build their own object detection neural network from scratch. This is research territory. Also this means you normally cannot take advantage of transfer learning as you do when using an existing architecture. So no. Go away.


# Process
So we have a PDF. Only a few tools above work with PDFs so we need to convert it to an image. It gets pretty big, 9000x9000 pixels. Most tools can’t even work with these big images, so a shrink operation is in place. 4000x4000 seems to work. I converted a few images, annotated them and trained a model using the simple Detecto framework in Colab. Then I tested with some images to get the model to detect my objects - no hits. What? Why? 

Let us consider what the input to a CNN (Convolutional Neural Network) is. It takes the pixel values of an image that is normally resized to 200-300 pixels depending on the exact model. Now if we should resize our 4000x4000 pixel image to 200x200 this is what we get.

![blur.png](/uploads/blur.png)

It is not a big mystery that our framework has trouble training from images looking like this! It is always a good idea to look at the same data thas the neural network sees and think:

- “What if I was the neural network, could I learn to recognize the objects in this image?” 

Ok so we cannot resize the images to the size the neural network wants, but if we split the big image into smaller parts then it should work. But doing a split implies a lot more work surrounding the inference step (when we ask the model to locate objects for us):

1. We must find coordinates of good splitting points, considering:
* Objects may be located exactly on the lines of the split so we need overlap of the suggested image parts
* All parts we create must be the same size 
2. We must decide how big our parts should be
* Smaller parts (closer to the 200-300 pixels of the neural network) means better accuracy but also more image parts to process for a single source PDF
* Bigger parts means less inference work, but also less accuracy
3. After we run all the image parts through the detection model we get a set of detected object for each part and we must process them to get rid of duplicates from the overlapping parts of the original image.
4. We must also translate all detected objects’ coordinates to the coordinates of the original image

So if we can avoid this image splitting it would be good. For this use case inference time has no hard real-time requirements and there is no other obvious workaround so we are fine. I did the split and trained a new model until I found a good size for the image parts, and now it worked much better. In this case “much better” means that the CNN says: 

-“Yeah I found something that could potentially be what you are looking for”

This is of course depending on the very limited number of images and annotations that I used. But at least now we have proved to ourselves that the small objects are detectable and can be found, which is progress. 


# Revisit the problem
With this new knowledge of the problem we talked some more to the customer. Turns out we only need to locate small snippets of text to start with, not actual pictures of things. Hmm. This makes our problem a lot more confined and maybe we can even move up the AI-stack! Time to try some of the “text detection” offerings. Many candidates are available for this problem:

* AWS Rekognition
* AWS Textract
* Google Vision API
* Azure Computer Vision

We run into some of the same problems with our large original images, but we now already know how to solve it by splitting. Sending in some of our image parts works pretty well. This is great news because we always want to create the simplest possible solution. Some other minor problems popped up using this new way to solve our detection problem, but generally we now have a solution to our core problem. Yay!

All that remains now is some “small” operational plumbing:

* How can the customer send in PDFs
* How can we trigger pre-processing
* Where is the pre-processing deployed
* How can we pass data to our model to do inferencing
* Where is the model deployed
* How do we retrain the model
* How do we monitor the model (concept drift)
* How do we monitor the system
* Where do we do post-processing
* How do we deliver the results back to the customer
* How do we orchestrate this entire flow
* Can the system scale
* How do we track and collect usage

These could all be topics for future posts.

# Summary
We discussed the specific use case of small objects in large images, how to approach it and how to consider the various options in all the layers of the AI-stack. We also learned that as we learn more around a problem domain during a project we can move up or down the AI-stack to find the simplest possible solution in the shortest amount of time. Now get out there and find those small objects!





