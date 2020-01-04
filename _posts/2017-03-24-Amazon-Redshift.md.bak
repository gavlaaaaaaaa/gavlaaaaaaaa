--- 
layout: post 
title:  Amazon Redshift at a Glance
author: Lewis Gavin 
comments: true 
tags: 
- cloud
- aws
---

![Amazon Redshit](../images/redshift.png)

This is a quick post following on with the AWS theme from the last two posts: [AWS Overview](http://www.lewisgavin.co.uk/AWSOverview) and [EC2](http://www.lewisgavin.co.uk/AWS-EC2). 

I wanted to delve into Redshift, that isn't something I've covered on the Udemy course I'm doing but is a technology I am particularly interested in from a "big data in the cloud" perspective.

## What is it?

Redshift is a scalable, cloud based, data warehousing technology that is marketed to be very cheap. It's architecture is very similar to that of Hadoop in that it has a Leader Node and 1 or more Compute Nodes.

Each of the Compute Nodes are split into slices. Each of the slices get allocated CPU and table data so that they can do work on the data in parallel. 

## Benefits

One obvious benefit is the scalability for both storage and processing. The fact that nodes can be split into slices that execute in parallel means that you can always scale your compute power horizontally.

Other benefits include:

1. **Columnar storage for faster computations**. If you are frequently performing column based calculations such as sums and averages then columnar storage will drastically reduce the amount of I/O because all the data items within a column are stored sequentially on disk next to each other.
2. **Column based compression**. Again this reduces I/O and means that each column can be encoded and compressed in isolation meaning you use less storage and therefore I/O.
3. **Zone Maps**. A zone map is in-memory block metadata. Contains things such as MIN and MAX values per block and will prune blocks that don't contain data for a given query. This again will minimize unnecessary I/O.
4. **Sort Keys for faster access**. A sort key will mean all files with similar keys are stored next to each other. If you access data through where clauses with date ranges, then using a date as part of your sort key will mean that only the nodes and blocks that contain those dates need to be accessed. All others can be skipped - heavily reducing I/O. This concept is similar to partitioning a Hive table in Hadoop.
5. **Distribution Keys**. This concept is very similar to the Row Key in HBase. By picking a distribution key that contains a lot of discrete values means that your data will be evenly distributed across the nodes (as similar keys get stored together). This is important for performance as if you have distribution keys that are too similar then a lot of data will be stored on one node (hotspotting). This means that parallel processing becomes difficult as all the data is on the same node, meaning one node is doing all the work.
6. **Inexpensive**. You pay as you go and you pay for the number of nodes x the price per hour.
7. **Continuous backups**. Similar to Hadoop, data is replicated across nodes meaning if a Node was to die, there is always at least 2 other nodes that contain the data. Also, because Redshift is within the AWS ecosystem it can make use of Amazons Hot/Warm/Cold storage options available in S3 meaning you can backup to S3 across regions (for increased safety).
8. **Fault tolerant**. Again like Hadoop if a Node or Disk goes down there are always others that can take over due to the replication. However even if a Region goes down, if you backup across regions you will be safe too.
9. **Security is built in**. Amazon VPC and [IAM](http://www.lewisgavin.co.uk/AWSOverview) for user access controls. All data is encrypted on disk and data can be loaded encrypted from S3 too.


