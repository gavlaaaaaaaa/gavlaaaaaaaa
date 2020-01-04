--- 
layout: post 
title:  Training a Machine Learning Model to Detect Sarcasm using Twitter
author: Lewis Gavin 
comments: true 
tags: 
- machinelearning 
- datascience
- spark 
---

![Sarcasm detector in Spark using Machine Learning](../images/sarcasm2.png)

In the last post a model was trained to detect sarcasm using just a small amount of sample data. In order for the model to be useful generically, in a larger amount of scenarios, it requires more training data that comes from a wider amount of sources. This is so the model understands many different types of sarcasm. Creating this data by hand would take a long time and would be suspect to personal bias. To solve this problem, twitter can be used to train the model by using any tweets that contain #sarcasm or #sarcastic as my sarcastic training data.

## What's changed?

The basic concept is to bring data in from twitter and get a consistent amount of tweets that are both sarcastic and non-sarcastic, so that the training data is fair. 

The tweets need to be labelled correctly too. If a tweet contains #sarcasm or #sarcastic it is labelled with a 1, otherwise it is labelled with a 2. The stream then continues to bring data in until there is a consistent amount of each.

~~~scala

    val stream = TwitterUtils.createStream(ssc, None)

    //label the tweets based on sarcasm hashtag
    val labelledTweets = stream.map(tweet => standardiseString(tweet.getText))
      .map{tweet =>
        if(tweet.contains("#sarcasm")) {
          (1, createNgram(tweet, 2))
        }
        else {
          (0, createNgram(tweet, 2))
        }
      }

~~~

Now we are pretty much at the same state as we were in [part 1](http://www.lewisgavin.co.uk/Sarcasm-Detector). We have a labelled data set that can now be vectorised and run through the same Naive Bayes model trainer as we did previously.

## Wrap Up

As you can see, this is a relatively shorter post than usual. The reason for that is that when it come to implementing the twitter training model, it turns out that most of the hard work was done in part 1. This meant that all I really needed to do was bring in some twitter data! A lot simpler than I expected when I started doing this.

However I thought it would be useful to at least share my findings and I may come back to update this post at a later date if I improve on the way it works.

To supplement the smaller post this week there is a [guest post](http://www.lewisgavin.co.uk/Vishal-EU) by a friend and colleague of mine - Vishal Jhaveri. Vishal used some big data analytics techniques - including using twitter data - to try and predict the outcome of the UK's EU referendum (better known now as brexit). Click the link above to find out more.

