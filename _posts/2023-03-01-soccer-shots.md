---
layout: post
title: Predicting Shots on Goal
author: Siobhan 
date: 2023-03-01 15:00:00 -0500
categories: [Online machine learning, Demo]
tags: [online machine learning, real time data, streaming data, orac]
youtubeId: uFC0YgpsVb8
img: arsenal.jpeg
---
![Flowing particles as a data stream](/arsenal.jpeg){:height="200px" width="400px"}

# Predicting shots on goal in real time


At Orac we have built a model to predict shots on goal up to 3 seconds in advance. The prediction is a rolling probability throughout the game, which serves as an indicator of the most exciting moments. We were inspired by this computer vision-based Soccer Goal Predictor built by Amazon to detect exciting moments that lead to goals. We wanted to achieve the same outcome with a specialized real time system, using only lightweight models and a stream of tabular data.


## Soccer


Soccer is a highly dynamic game; the ball moves roughly 12 miles during a game with average ball speeds reaching 70 mph. Shots on goal are rare and goals themselves are far rarer. Soccer has enthralled generations around the world due to the fast paced, highly skillful and unpredictable nature of the game.


At Orac we are especially attracted to fast moving and incredibly varied data for real time predictions. These are the most challenging scenarios in which to build robust     machine learning models with high levels of accuracy. Our system is designed for just this kind of scenario.




## Real Time Soccer Data


We used a data stream from statsbomb which provides real time on ball metadata. For this use case we have not incorporated any other data source which might be beneficial (historical team or player statistics, in-game image stream).



{% include youtubePlayer.html id=page.uFC0YgpsVb8 %}


## Building the model


We pre-trained a model on one game of football. That model was the foundation for both an experimental sandbox model and the production model. The sandbox model is exposed to the same production data stream, it is both scoring and learning with each incoming data point. The production model is scoring only and is replaced by the sandbox model every 60 seconds. The purpose of the sandbox model is to have a representative model which is entirely up to date with current patterns on the stream, while protecting the production model from instability in learning. 


## Orac Results


The features for the model are all generated within Orac in real time. Online transformations (real time features) generated within Orac enable us to calculate momentum and attacking moves as they unfold. The target variable is continuous, it is generated from shots on goal which are encoded as 1. The position of the ball on the field with respect to an attacking or defensive possession is then used to generate the range of values between 0 and 1. Finally this is smoothed using centered moving averaging.

The model is a nearest neighbor regressor, holding minimal records in memory to ensure the model is performant for real time updates and inference. In order to optimize the hyperparameters the number of neighbors used and the number of records retained in memory were analyzed. A smaller window size, holding 100 previous data points in memory outperformed larger ones for both efficacy and model speed and 25 neighbors were used for proximity calculations. 


![Hyperparameter search](/soccer_hyperparameter.png){:height="200px" width="400px"}


With this model we achieved an MSE of 0.033. Historical data on team and players would surely help as would real time image based data, however the purpose here is to display the power of online machine learning, to create something scalable and efficient while achieving real time predictions in a highly dynamic environment.


![MSE](/soccer_mse.png){:height="200px" width="400px"}
