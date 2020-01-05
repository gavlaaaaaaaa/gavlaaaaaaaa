---
layout: post
title: What is a Data Engineer?
author: Lewis Gavin
comments: true
tags:
- aws
- redshift
- bigdata
- dataengineer
---

![Photo by [Franck V.](https://unsplash.com/@franckinjapan?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)](https://www.lewisgavin.co.uk/images/whatisdataengineer.jpg)
Photo by [Franck V.](https://unsplash.com/@franckinjapan?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

The job title Data Engineer doesn’t always come with the same sexy connotations as something like Data Scientist. However looks aren’t everything and the work that a Data Engineer does, actually makes up a significant portion of the work carried out by a Data Scientist.

Understandably, topics like Machine Learning and AI are always going to win the popularity contest, especially as they grow in popularity in mainstream media. However a good chunk of the work that sits behind these concepts stems from Data Engineering work.

Now this isn’t an article about the battle of Data Engineers vs Data Scientists, there’s no beef here. Instead this article comes off the back of the sea of articles I’ve seen recently talking about this exact point: that 80% of a Data Scientists work is data preparation and cleansing.

So I’m going to talk about why I feel Data Engineering is an important field based on what it enables and how together, with Data Science, provides the backbone to the latest tech in the industry.

#### AI and Machine Learning models require data

Talk to any Data Scientist and they’ll tell you that obtaining data, especially a source that has absolutely everything they require for their model is a distant dream.

In the real world, data sets that are this helpful are a rare thing and that’s where the first skill of the Data Engineer comes into play. We dedicate a lot of our time to pulling data sets from an array of sources, most of the time into a central hub to be utilised together.

![Image Source: [https://i.redd.it/2c2megoon2411.jpg](https://i.redd.it/2c2megoon2411.jpg)](https://cdn-images-1.medium.com/max/800/0*SB_oUFoIfihKL0Mf.jpg)
Image Source: [https://i.redd.it/2c2megoon2411.jpg](https://i.redd.it/2c2megoon2411.jpg)

Now anyone can go and download a static data set from a website. The benefit of a Data Engineer is just that: the engineering. Not only can they provide you a multitude of data from disparate sources, they can do it in a way thats repeatable, frequently updated and if required, even in real-time.

#### Clean Data == Better Model

All the data required to build your AI or ML model is now coming in frequently to your central data hub (I wrote a recent article on [**how to architect the perfect data warehouse**](https://medium.com/@lewisdgavin/how-to-architect-the-perfect-data-warehouse-b3af2e01342e) if you want more detail on this).

The next step required before getting your AI and ML model off the ground is preparing the data. Now this is where this article stemmed from for me. The sheer amount of articles I’ve seen about this and even a [survey from Forbes in 2016](https://www.forbes.com/sites/gilpress/2016/03/23/data-preparation-most-time-consuming-least-enjoyable-data-science-task-survey-says/) is just insane.

![[Image Source: Forbes](https://www.forbes.com/sites/gilpress/2016/03/23/data-preparation-most-time-consuming-least-enjoyable-data-science-task-survey-says/)](https://cdn-images-1.medium.com/max/800/0*Jt2gTgADOPt7hdsF.jpg)
[Image Source: Forbes](https://www.forbes.com/sites/gilpress/2016/03/23/data-preparation-most-time-consuming-least-enjoyable-data-science-task-survey-says/)

The Forbes survey states that 80% of Data Science work is data prep and that 75% of Data Scientists find this to be the most boring aspect of the job.

👋 Guess what, this is where the Data Engineer also thrives. We spend so much time manipulating data that a lot of this work comes as second nature to us. Whether it’s joining data sets together, cleaning out null and erroneous values, manipulating strings into features or aggregating data we’ve got you covered.

As a freebie, again all of this will be built in a repeatable way so as data is updated its also cleansed providing a consistent source of fresh, clean data.

And as a two for one you also get the added benefit of freeing up your Data Scientists to squeeze every last inch out of the model and keep their morale higher as the most boring part of their job has now disappeared.

#### Finally building a model

We’re finally there, after all this upfront effort work on the model can finally begin and you might think this is where the Data Engineer disappears into the abyss.

However, anyone who’s built AI and ML models will know that life isn’t that simple. As the model is being built, multiple iterations of the above will be cycled through as more questions are asked and extra data required for answers.

This is where the Data Scientist really shines though and I can’t stress enough how important it is to let them do their thing here. Everything I’ve talked about so far isn’t me trying to say that Data Engineers are better or worth more, it’s to show how they can enable more efficient workflows for Data Scientists to get down to the nitty gritty.

#### A model is only useful if someones going to use it

Work is complete on the first iteration of the model. We can all pack up and go home right? As most of you already know, this isn’t the case. The model might be built but there are few things left to consider: how will it be used in the real world and how quickly will it become stale.

The AI or ML models purpose was to solve a problem in the real world so it now needs to be applied. Usually this means being implemented into an application or maybe its used for segmentation or predictive marketing.

![Image Source](https://cdn-images-1.medium.com/max/1600/1*jBjJw7jq71pw4iZkqyp2Uw.jpeg)

A Data Engineer will be able to add the model to a data pipeline that processes your whole user base against the model and segments them accordingly. They can the use this to trigger automated comms or publish the segments to a data layer to enable segment targeted content on your website.

And if you’re worried that the model will become stale in a few months time don’t fear. A good Data Engineer will be able to work with the Data Scientists and translate their work into something that can be continuously updated. Feeding in new data, rebuilding the model and automatically publishing it.

#### So what am I getting at?

Whether you’re considering hiring a Data Engineer or are looking at getting into the field of data but don’t know where to start it’s clear to see that Data Engineering is an important field to consider.

You can see just how much of the work behind building a data model and making it live can be accomplished by a Data Engineer.

The efficiencies gained mean that getting to the point of building the model will be quicker and the models will undoubtedly be better, as the Data Scientists have more time to spend tweaking and improving them.
