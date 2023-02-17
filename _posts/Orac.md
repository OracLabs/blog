---
title: Orac
date: 2023-02-17
categories: [blog]
tags: [online machine learning, real time data, streaming data, orac]
---

# Machine learning for real time data streams

Orac is the first solution to optimize model training, experiementation and deployment for real time data environments.


## Real time data 

![flowing-particles](flowing-particles.jpg){: w="50" h="100" }

The vast majority of MLOps solutions are designed for batch processes where models must be trained offline rather than in real time. Yet,real time data streams underly some of our most valued applications, such as fraud detection, network classification, IOT, e-commerce, streaming entertainment, social, etc. 



## Traditional machine learning
In a traditional batch process a model learns patterns from a sample of historical data and is then deployed to the stream to make inference on real time data. Very quickly the patterns on the stream diverge from what was learned on historical data and models fall out of date.


## Orac
With Orac a model can be launched directly from a notebook to continue learning from real time data. A model can be monitored as it is continuously learning in a safe sandbox where multiple models can eb A /B tested in real time. From there a model that is performing well can be deployed to production for inference only, while experimentation continuous in the sandbox in the background. With Orac models never fall out of date and there is no need to retain as models are continuoulsy updating and learning current trends from the data stream. 