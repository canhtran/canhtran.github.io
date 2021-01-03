---
title:      "How to pass the Azure Data Scientist Associate Test"
date:       2020-07-06
author:     "Canh Tran"
categories: "DATA SCIENCE"
layout:     "post"
---

** This article covers the most recent exam syllabus. I took the exam in 13th June 2020 **

In this guide, I will explain the idea of Azure Data Scientist Associate, the content of the exam, preparations, and some keys take away after the course.

![my-cert](https://miro.medium.com/max/1400/1*ou4Aj2xOs-90zkPCLEQBdw.png)

I spend roughly 50 hours to learn from almost zero knowledge about Azure to get the certificate. About my background, I have 2 years working as a Data Scientist and now I’m a full-time Data Engineer with 3+ years experience. Being on both sides, I’m familiar from set up infrastructure for large scale data to build and deploy real-time models to production. I think this is one of the advantages that help me pass the exam.

## What is the Microsoft Azure DP-100 exam?

Before getting to the details, I would like to recap about the Microsoft Azure DP-100. “Designing and Implementing a Data Science Solution on Azure” (or DP-100) is the exam you need to take to get the Azure Data Scientist Associate. Based on Microsoft, this course tests your knowledge of data science and machine learning to implement and run machine learning workloads on Azure. There are 4 skills measured in the exam

- Setup an Azure Machine Learning workplace (30–35%).
- Run experiment and train models (25–30%).
- Optimize and manage models (20–25%).
- Deploy and consume models (20–25%).

More details, the exam has 55 questions (10 of them cannot be reviewed). The questions are categorized into 5 different types

- Multiple choice single answer
- Multiple choice multiple answer
- Arrange in the correct order
- Scenarios based questions
- Complete the code by fill in the blank

Oh, I almost forgot to mention that the exam took 180 minutes and able to take at home.

## How do I prepare for it?
![Photo by Rishabh Agarwal on Unplash](https://miro.medium.com/max/1400/1*eQiIx2AypOXM4FEzVS6Xdg.jpeg)
*Photo by Rishabh Agarwal on Unplash*

All of my learning resources are free from Microsoft. When I first sign up for the free account, Microsoft gives 200$ credits for 30 days. That enough to practice for the exam, actually I spent hardly around 50$ on it.

Since I have little knowledge about Azure, I’m decided to follow the DP-100 lab (https://github.com/MicrosoftLearning/DP100/tree/master/labdocs) provided by Microsoft on GitHub. It took me around 20 hours to finish the lab. The lab is divided into 10 modules but I can say it covers two major tools

- Azure Machine Learning Designer, which is a drag and drop tool that you can use to build, test, and deploy the predictive analytics solution.
- Using Azure Machine Learning SDK for Python to build, run, and deploy machine learning workflows with Azure Machine Learning services. You can interact with the service in any Python environment with the SDK.

While taking the lab, you need to pay attention to understand each line of the code. There are a lot of “fill in the code” questions, so you have to remember it. I was struggling a lot during the exam because most of the answers are somehow similar to each other.

For the rest of the time, I try to understand the Azure Machine Learning Studio by reading and following the official documentation (https://docs.microsoft.com/en-us/azure/machine-learning/overview-what-is-azure-ml). I only found the documentation while I was searching for a bug in my code. Fortunately, it really helps me for the exam, most of the questions are from the documentation. The docs cover every aspect of the Studio which doesn’t include in the lab. For example, model training with PyTorch or time series forecasting or even user permission for Datastore.

Overall, the updated exam is quite short and it’s mainly covered the Azure Machine Learning Studio. From what I remember, there are around 3–5 questions required the data science knowledge likes tuning parameters or choose the right parameters. Although, it’s basic knowledge of data science.

That’s what I did for prepare the exam. I’m not suggesting that my approach is a correct way to do but if you haven’t found any materials or other ways don’t work, feel free to use mine.

## Keys takeaway after the exam

From my perspective, the Azure Data Scientist Associate is useful for someone who has knowledge about data science or at least has trained a couple of predictive analytics models.

Through the study, we learn the whole process of training the model, building a pipeline for experiment and deploy the model to production in an efficient way. In my opinion, it’s not only good for the Azure Platform but also helps you have the idea of real-world processes even on other platforms.

I hope this guide has been helpful and a hopefully a confidence booster for those taking the exam soon

I wish you the best of luck!