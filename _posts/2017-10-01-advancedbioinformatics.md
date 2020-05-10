---
layout:     post
title:      Advanced Bioinformatics
subtitle:    
date:       2017-10-01
author:     Junjie Hua
header-img: img/post-bg-o.jpg
catalog: true
tags:
    - Lecture
---

## Why

- ##### If you can't speak in a classroom, venue, etc., you want to interact without speaking out
![fig2](https://edmond123456.github.io/img/advancedbioinformatics/2.jpg) 

- ##### Tell the story without pronunciation
![fig3](https://edmond123456.github.io/img/advancedbioinformatics/3.png)


## Background
- ##### Researchers at Oxford University and Google DeepMind have developed artificial intelligence (AI) that reads the movement of a person's lips through training with thousands of hours of content broadcast by the BBC.

- ##### Little research on facial muscle activity

- #####  Difficult to discriminate micro electromyographic activity

- ##### Obtained action potential is a continuous waveform that fluctuates in time and is difficult to analyze usually.

## How
##### Different mouth shape when speaking different vowels
![fig6](https://edmond123456.github.io/img/advancedbioinformatics/6.png)

##### By collecting masticatory EMG data when speaking different vowels(chin--plus,cheek--minus,forehead--reference)
![fig5](https://edmond123456.github.io/img/advancedbioinformatics/5.jpg)

## Result
- ##### I analyzed the EMG data by analyzing envelope of different signals, different signals can be easily distinguished 
![fig4](https://edmond123456.github.io/img/advancedbioinformatics/4.png)

- ##### By using a simple neural network from MATLAB, different EMG signals of different vowels can be correctly distinguished.
![fig1](https://edmond123456.github.io/img/advancedbioinformatics/1.png)