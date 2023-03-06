---
layout: post
title: Predicting Shots on Goal
author: Siobhan 
date: 2023-03-01 15:00:00 -0500
categories: [Online machine learning, Demo]
tags: [online machine learning, real time data, streaming data, orac]
youtubeId: uFC0YgpsVb8
---

# Predicting shots on goal in real time

At Orac we have built a model to predict shots on goal up to 3 seconds in advance. The prediction is a rolling probability throughout the game, which serves as an indicator of the most exciting moments. We were inspired by this computer vision-based Soccer Goal Predictor built by Amazon to detect exciting moments that lead to goals. We wanted to achieve the same outcome with a specialized real time system, using only leightweight models and  a stream of tabular data.

{% include youtubePlayer.html id=page.uFC0YgpsVb8 %}

## Soccer

Soccer is a highly dynamic game, the ball moves roughly 12 miles during a game with average ball speeds reaching 70 mph. Shots on goal are rare and goals themselves are far rarer. Soccer has enthralled generations around the world due to the fast paced, highly skillful and unpredictable nature of the game. 

At Orac we are especially attracted to fast moving and incredibly varied data for real time predictions. Those are among the most challenging problems to host and for which to build robust machine learning models with high levels of accuracy. Our system is designed for just this kind of scenario. 


## Real Time Soccer Data

We used a data stream from statsbomb which provides real time on ball metadata. For this use case we have not incorporated any other data source which might be beneficial (historical team or player statistics, in-game image stream). 



## Building the model

We pre-trained a model on one prior game of football. That model is the initiating state for both an experimental sandbox model and the production model. The sandbox experiemntal model is exposed to the same production data stream, it is both scoring and learning with each incoming data point. The production model is scoring only. The production model is replaced by the experimental model every 1 minute. The purpose of this is to have a representative model which is entirely up to date with current patterns of the game, while we maintain the separation of the production model in case of any instability in learning. 

## Orac Results

For the features, online transformations with Orac enable us to calculate momentum and attacking moves as they unfold. The target variable is continuous, it is generated from shots and a3 second backfill of shots. The position of the ball on the field with respect to an attacking or defensive possession is then used to generate the range of values between 0 and 1. Finally this is smoothed using a centered moving averaging. 

The model is a nearest neighbor regressor, holding minimal records in memory to ensure the model is performant for real time updates and inference. 

In order to optimize the hyperparameters the number of neighbours used and the window size to retain in memory (‘recency bias’ or ‘model forgetfulness’) were analyzed. A smaller window size, holding 100 previous data points in memory outperformed larger ones for both efficacy and model speed and 25 neightbors were used for proximity calculations. The mean of those generated our predicted goal probability. 

![Hyperparameter search](/soccer_hyperparameter.png){:height="200px" width="400px"}

With this model we achieved a  MSE of 0.033 Batch data on team and players would surely help as would image based data, however the purpose here is to display the power of online machine learning  to create something scalable and efficient while achieving real time predictions in a highly dynamic environment.

![MSE](/soccer_mse.png){:height="200px" width="400px"}


#