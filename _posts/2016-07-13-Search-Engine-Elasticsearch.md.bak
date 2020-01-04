---
layout: post
title: Building a search engine with Elasticsearch
author: Lewis Gavin
tags:
- elasticsearch
- bigdata
- searching
---

![Building a Search Engine](../images/building-search-engine.jpg)

## What is Elasticsearch?

Elasticsearch is real time search and analytics engine. It indexes documents at a large scale and allows you to search them and perform analytics on them quickly.

Elasticsearch can be integrated with Hadoop to allow the collaboration of big data storage and big data searching and analytics. In this post I will give a brief overview of how it works. Then I'll delve into some examples based off a recent project.

## How Does it work?

At its core, Elasticsearch uses Lucene. Lucene is a Java engine built to optimize the storage of text. It can then efficiently search and retrieve text items that closely match search terms.

*So why the need for Elasticsearch?* Well Elasticsearch gives you a cleaner API to the lower level Lucene engine, it is much more scaleable and supports plugins and integration with a bunch of other technologies.

**The key concepts** can be broken down into 3 main areas. The first key concept is the **index**. An index is simply a collection of documents. These documents should be of a similar style such as product information details and reviews or software documentation. This index is then how you add, modify and retrieve your documents.

The second concept is **types**. Types sit underneath an index and act as logical seperation of components within documents stored in an index. For example, lets say you are building an application that stores software documentation. Within the application each documentation page contains the text content, any images and also any comments. This whole page would be under the same index, but types should be used to seperate out each component. This gives some logical separation when searching an index.

The final one is the **shard**. An index cant be split up, so if you want to distribute it across nodes in a cluster this may appear to be a problem. The way Elasticsearch solves this is by using multiple Lucene indexes that it calls shards. So a shard is just a Lucene index.

## Hadoop Integration and Benefits

You've got a few documents, you want to index them and then perform some searches. This is great, however in enterprise this isn't likey to be your use case. You're more likely to have petabytes of data stored in a cluster of some description, some of which could benefit from elastic search.

![ES-Hadoop](https://static-www.elastic.co/assets/bltc0e7de6e02236a46/eshadoop-diagram.png?q=935)
*Image from https://www.elastic.co/products/hadoop*

Elasticsearch have made this pretty simple by building [elasticsearch-hadoop](https://www.elastic.co/products/hadoop). It solves the distributed paradigm that comes with Hadoop by mapping Hadoop InputSplits or if you're using Spark, a partition, to ES shards. This means you can reap the benefits of distrubuted storage and still perform tasks on your data. Data is also co-located where possible as ES-hadoop will talk to Hadoop and Spark. This will prevent unneccesary data transfer over the network.

## Building your first Search Engine

Blah blah blah... I know - **so how do you actually use it?** Elasticsearch as a really simple REST api that you can use to *post* documents into an index and also *get* them out using an array of search parameters.

~~~bash
$ curl -XPUT 'http://localhost:9200/blog/page/1' -d '{
    "author" : "Lewis",
    "post_date" : "2016-07-15T10:00:00",
    "content" : "This is my blog post on elasticsearch..."
}'
~~~

**Boom** - first index created. So lets break down what this means.
Within the URL after the host and port we have three items. **blog** is the name of the index, **page** is the *type* that we discussed earlier, and the number **1** is simply the ID.

This is what would be returned as the result.

~~~bash
{
    "_shards" : {
        "total" : 5,
        "failed" : 0,
        "successful" : 5
    },
    "_index" : "blog",
    "_type" : "page",
    "_id" : "1",
    "_version" : 1,
    "created" : true
}
~~~

This just clarifies what we've just said above and also shows the number of shards/replications created. 

So our document is now stored, we can simply repeat this process for as many blog posts as we create. **So how do we get documents out**

There's an easy way, and a more advanced way. I'll show you an example of both!
**EASY**

~~~bash
curl -XGET 'http://localhost:9200/blog/_search?q=author:lewis'
~~~

**ADVANCED**

~~~bash
curl -XGET 'http://localhost:9200/blog/page/_search' -d 
'{
    "query" : {
      "term" : { "author" : "lewis" }
    }
}'
~~~

Here is what would get returned back as a result.

~~~bash
{
    "_shards":{
        "total" : 5,
        "successful" : 5,
        "failed" : 0
    },
    "hits":{
        "total" : 1,
        "hits" : [
            {
                "_index" : "blog",
                "_type" : "page",
                "_id" : "1",
                "_source" : {
                    "user" : "Lewis",
                    "post_date" : "2016-07-15T10:00:00",
                    "content" : "This is my blog post on elasticsearch..."
                }
            }
        ]
    }
}
~~~

There are a bunch of different parameters and a vast array of different search techniques that can be used. All of which can be found in the [official documentation](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-request-body.html)

## Wrap Up

Hopefully this little taster into ES was useful and demonstrated how you can quite quickly start to build your own search engine! All it takes is a little imagination and a simple web front end and you'll be competing with google in no time. I'm hoping to do another post on elasticsearch shortly demonstrating how to index documents at scale using Kafka, Spark and Elasticsearch.

