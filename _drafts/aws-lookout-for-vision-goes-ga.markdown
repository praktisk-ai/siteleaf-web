---
title: AWS Lookout for Vision goes GA
date: 2021-03-04 15:04:00 +01:00
image: "/uploads/pcb.jpg"
---

A few days ago AWS announced General Availability for their service Lookout for Vision. This is an AI service that provides image based anomaly detection in manufacturing using computer vision. With basically no need for AI knowledge this service can be quickly implemented in your manufacturing process to detect product anomalies and defects at a low cost. Using this service would increase product quality as well as reduce operational costs. The service was released in beta during AWS re:invent 2020 in the beginning of December.


## How it works

AWS Lookout for Vision (LFV) works by training on as little as 30 images. From these images the model will learn what the normal “state” looks like. Let us take the example of printed circuit boards (PCB). In this case the model would learn the normal layout and look of a PCB. If a previously unseen new image of a PCB would show a scratch or perhaps a missing component the AI would classify this as an anomaly, i.e. differing significantly compared to the expected look of a PCB. 

Using computer vision for quality control is nothing new under the sun. However, this has traditionally required specialists to set up and carefully tune the system for a single product. If anything should change, or if additional product lines should also be quality controlled, the specialist would have to come back and redo the whole thing. With LFV this re-tuning can be largely automated, small gradual changes to the products will also be learned by the model as the “new normal” state and can be made to not trigger anomaly alerts. It is the rapid or big changes that should trigger alerts. So this will allow a wider and faster adoption of quality control across an entire company.

## Getting started

To get started on a prototype project using LFV you just need images of the products you want to work with. All GUIs and APIs are available for you as part of the provided service. You will do all labeling work as well as monitoring of the running project in the AWS Console. This way you can verify if LFV is for you and that it meets your business requirements.

After you have done the basic verification of your use case the next step is probably a bit bigger. Now you will need to deliver images continuously, or as frequently as you like, to LFV for anomaly classification. This requires you to connect and communicate with cameras, existing or new ones, in your factory. So this means quite a bit more integration work both regarding hardware and software. But this should not stop you if you already verified the value of the project during the prototyping phase.


## Cool use case: Pizza

![pizza2_box.jpg](/uploads/pizza2_box.jpg)

One Swedish company that produces different kinds of ready-made foods, like pizzas, are using LFV. They use it on their production line for frozen pizzas to make sure the quality standards are met. This solution can detect for instance if the cheese coverage is insufficient or if any large component, like a pepperoni, is missing. I think this is my favorite AI use case so far! 

Torbjörn Stavenek

