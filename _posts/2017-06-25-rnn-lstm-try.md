---
layout:     post
published: false
title:      "Experiment RNN and LSTM with Sherlock Holmes"
subtitle:   "Using LSTM Recurrent Neural Network to do text generation"
date:       2017-06-25
author:     "Canh Tran"
header-img: "inblog/20160831/xdata.png"
tags:
    - Deep Learning
    - LSTM
    - RNN
---

Hey, These days I'm quite boring so I find something that I can play around. One of my colleage told me about LSTM and RNN with its power of generating the text. Immediately, wow it's awesome, I will try it.

Then I'm starting to google and found this article [here](http://machinelearningmastery.com/text-generation-lstm-recurrent-neural-networks-python-keras/) by Jason Brownlee

I have to said that I don't know anything about the RNN (Recurrent neural network) or LTSM, so I just write the code exactly in the article. But instead of using "Alice in Wonderland", I use the 3 stories from "The Adventure of Sherlock Holmes" for trainning set.


Here is my first attempt result:
It tooks 14 hours on my big data machine (CPU only). Oh Gosh, but it's so excited right ?

```
Seed:
most outré results, it would make all fiction with
its conventionalities and foreseen conclusions
would see that the could soart his face.
```

```
"well, i was always dome to the coor of the manees to a parrinn for the oastage in the manter of the mantern and a sling sige the door and takk to the coore of the mantert and had been way alay with a senden for street, and the man who was a lan who manged on the coor, and the cond was suill fallly into the coorer. "i have not be all to be and tee meat up the shouter of the manees of the maneer to a parring man wooked up to the coor of the mantert and had tooe that the would not be olees of the red-headed league offices of the sooe and langeing for my own sone toart that the could brake in the stept which was the sherl with the stoeet which was not on the steps which was not to the coor of the mantert of his pwn his crnce of the rtoeet.
"the man she man shat i had not see him that i had not see him to see menter. and the forrer when i had see her mentinn to his hands in his hands when he was a small sadle the sooee of the sooee and a slieh of
```

Well it can be readable but not what I want to achieve. I need to change something to make it like a "real" story. But first of all I must understand about LSTM and RNN.

I spend around one week on the internet to read and experience with LSTM and RNN. In this mean time, I also increase the epoch to 100 and add one more layer to the code and let's running (It tooks a quite time to run for sure).



<List of code modification>
<Reason why I modify it>

The second time running took almost 5 days on the same machine.
<Result here>




