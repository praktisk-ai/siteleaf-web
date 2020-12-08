---
title: AWS Panorama (re:Invent 2020)
date: 2020-12-08 11:46:00 +01:00
published: false
image: "/uploads/pano.jpg"
---

This is computer vision at the edge. When you have video streams that you need to analyze with some of the following requirements: real-time responses, the geographical location has bandwidth limitations or intermittent connectivity, or regulations constrain what you may send to the cloud, then Panorama could be for you. This is basically “computer vision” in a box with a console and an SDK. 

Use cases could be anything vision related like people counting, face detection, person distance alerting, personal protection equipment detection, etc. 

You develop your ML-models in Sagemaker or elsewhere (but in a supported framework). You also develop your actual business logic in a serverless lambda function and deploy them on the physical Panorama hardware appliance using Sagemaker Neo and IoT Greengrass. Then you have to connect your on-prem cameras to the appliance and - your are done.

The nice thing about Panorama is that you get a setup ready to start developing with, nicely integrated with many existing AWS services. Even though the hardware is prepared for you and the Panorama console comes with a number of predefined AI models you still need to do a bit of work to actually get it up and running in a physical environment. But many obstacles have been removed for you, so it will still be much faster than starting from zero.


Read more here:

[https://aws.amazon.com/panorama/](https://aws.amazon.com/panorama/)

