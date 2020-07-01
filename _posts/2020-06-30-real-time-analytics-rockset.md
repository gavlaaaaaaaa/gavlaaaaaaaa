---
layout: post
title: Real Time Analytics with Rockset
author: Lewis Gavin
comments: true
tags:
- rockset
- realtime
- bigdata
---

![data engineering real time analytics](https://www.lewisgavin.co.uk/images/data-engineering-real-time-analytics.jpg)

As a data engineer, my time is spent either moving data from one place to another, or preparing it for exposure to either reporting tools or front end users. As data collection and usage have become more sophisticated, the sources of data have become a lot more varied and disparate, volumes have grown and velocity has increased.

Variety, Volume and Velocity were popularised as the three Vs of Big Data and in this post I’m going to talk about my considerations for each when selecting technologies for a real time analytics platform, as they relate to the three Vs.

## Variety
One of the biggest advancements in recent years in regards to data platforms is the ability to extract data from storage silos and into a data lake. This obviously introduces a number of problems for businesses who want to make sense of this data because it’s now arriving in a variety of formats and speeds.

To solve this, businesses employ data lakes with staging areas for all new data. The raw data is consistently added to the staging area and then picked up and processed by downstream processes. The major benefit to having all the data in the same place means that it can be cleaned and transformed into a consistent format and then be joined together. This allows businesses to get a full 360 degree view of their data providing deeper insight and understanding.

A data warehouse is often the one place in a business where all the data is clean, makes sense and in a state ready to provide insight. However, they are often only used within the business for daily reports and other internal tasks, but are rarely exposed back to external users. This is because if you want to feed any of this insight back to a user of your platform, the data warehouse isn’t usually equipped with the real time speed that users expect when using a website for example. Although they are fast and capable of crunching data, they aren’t built for multiple concurrent users looking for millisecond-latency data retrieval.

This is where technologies like Rockset can help.

Rockset is a real time analytics engine that allows SQL queries directly on raw data, such as nested JSON and XML. It continuously ingests raw data from multiple sources--data lakes, data streams, databases--into its storage layer and allows fast SQL access from both visualisation tools and analytic applications. This means that it can join across data from multiple sources and provide complex analytics to both internal and external users, without the need for upfront data preparation.

![data engineering real time analytics](https://www.lewisgavin.co.uk/images/data-engineering-real-time-analytics.jpg)


Traditionally, to do this with Amazon Redshift, you would have to build data pipelines to crunch the data into the exact format required to be shown to the user, then copy this data to DynamoDB or similar and then provide access to it. Because Rockset supports rapid SQL on raw data you don’t need to crunch all the data upfront before copying it, as transformations and calculations can be done on the fly when the request is made. This simplifies the process and in turn makes it more flexible to change later on.

## Volume

Data platforms now almost always scale horizontally instead of vertically. This means if more storage or power is needed, new machines are added that work together instead of just increasing the storage and power of a single machine.

A data warehouse will obviously require a lot of storage space due to it storing all or the majority of a business’s data. Rockset typically will not be used to hold the entirety of an organisation’s data but only its unstructured data and the subset required for real time requests, thus limiting the amount of data it needs to store.

And if you are planning on copying huge amounts of data to Rockset, this also isn’t a problem. Rockset is a cloud based solution that is scaled automatically based on how much data is copied to the platform and you only pay for how much storage you use. It’s also built to serve complex queries on large volumes of data, using distributed query processing and a concept known as converged indexing, so that query times remain fast even over terabytes of data.

## Velocity

The volume of data being stored is ever increasing due to the velocity at which it is being created and capture. Real time streaming technologies such as Apache Kafka have allowed businesses to stream millions of rows per second from one data source to another.

You may be contemplating streaming data into a data warehouse and querying it there, but Rockset provides a different model for accessing these streams. Kafka connectors are available within Rockset to consume streams from Kafka in real time. This data will be immediately available for querying as SQL tables within Rockset, without requiring transformation, and queries will use the latest data available each time they are run. The benefits of this are huge as you are now able to realise insight from data as it’s being produced, turning real time data into real time insight, instead of being delayed by downstream processes.

Another benefit of using Rockset is the ability to query the data via APIs and due to its ability to serve low-latency queries, these calls can be integrated into front end systems. If the velocity of your data means that the real time picture for users is always changing, for example users can comment and like posts on your website, you’re going to want to show in real time the number of likes and comments a post has. Every like and comment logged in your database can be immediately copied into Rockset and each time the API is called it will return the updated aggregate numbers. This makes it incredibly easy for developers to integrate into an application due to the out of the box API provided by Rockset. This just wouldn’t be possible with traditional data warehousing solutions.

## How Data Engineers Can Use Rockset

If your business doesn’t have a data warehouse, then for fast and rapid insights on your data, I would recommend pulling this data directly into Rockset. You can quickly get to insights and allow other members of the team to utilise this data which is vital in any business, even more so in a new startup.

If you already have a data warehouse then you will probably find that for most of your daily business reports, the data warehouse will suffice. However the addition of Rockset to take your raw data in real time, especially if you are a web company producing web logs, registering new users and tracking their behaviour, will give you a real time view of your data too. This can be powerful when you want to feed this data back to front end users, but also to allow your internal teams to monitor performance in real time and even spot potential issues as they arise instead of a day later.

Overall I would say that Rockset ticks all the boxes for dealing with variety, volume and velocity. Data engineers often spend a lot of time getting all the business data clean, correct and prepared for analysis within a data warehouse however it often comes with some delay. For times when you need real time answers, Rockset simplifies the process of making this data available to end users without the overhead required by other solutions.

