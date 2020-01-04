--- 
layout: post 
title:  Data Ingestion with Kafka and Spark, Big Data Journey Part 2
author: Lewis Gavin 
comments: true 
tags: 
- docker 
- hadoop
- pentaho 
---

![Data-Ingestion-Kafka-Spark-Part2](../images/bdj-part2.png)

Welcome to part 2 of the series. In [part 1](http://www.lewisgavin.co.uk/CDH-Docker) we looked at installing a CDH quickstart docker container along with Pentaho BA. Now we have some basic infrastructure in place, it's time to start thinking about data!

The logical place to start is **data ingestion**. You can't do anything until you have some data, so in this post we are going to explore using Apache Kafka to stream data in real time from Twitter. If you are not familiar with Kafka I recommend [this post](http://www.lewisgavin.co.uk/Streaming-Kafka/) to outline the basics before continuing.

## Installing Kafka

Unfortunately Kafka does not come pre-installed on our CDH instance so we will need to install it. It is important that we install version **0.9.0** or above, as this will allow us to integrate it with Spark streaming a little later.

You can get all the details of how to do this from [Clouderas online documentation](https://www.cloudera.com/documentation/kafka/latest/topics/kafka_packaging.html)

Once you've made sure you're pointing at the latest parcels simply install kafka:

~~~bash
$ sudo yum install kafka
$ sudo yum install kafka-server

~~~

To ensure you have the right version go to the Kafka libs file `cd /usr/lib/kafka/libs` and there should be a file with a name something like `kafka_2.11-0.9.0-kafka-2.0.2.jar` - the 0.9.0 in the name indicates the Kafka version.

**Now for some important config changes**. Within `/usr/lib/kafka/config` edit the file called `server.properties` and make sure you add/amend the following:

Ensure the broker.id property is given a number (I recommend 0)

~~~bash
############################# Server Basics #############################                
                                                                                         
# The id of the broker. This must be set to a unique integer for each broker.            
broker.id=0

~~~

Set the port to 9093. **The default is 9092 but that is taken by Pentaho BA**

~~~bash

# The port the socket server listens on
port=9092   

~~~

Finally, make sure the hostname and advertised hostname are set to localhost

~~~bash

# Hostname the broker will bind to. If not set, the server will bind to all interfaces   
host.name=localhost    
                                                                                         
# Hostname the broker will advertise to producers and consumers. If not set, it uses the 
# value for "host.name" if configured.  Otherwise, it will use the value returned from
# java.net.InetAddress.getCanonicalHostName().          
advertised.host.name=localhost  

~~~

**Lets start Kafka!**

~~~bash

$ service kafka-server start

~~~

### Testing Kafka

There is a nice quickstart/hello world that comes with the Kafka installation that you can walk through using the [kafka quickstart guide](http://kafka.apache.org/07/quickstart.html). 

~~~bash

# Run the producer to take input from standard input
$ kafka-console-producer --broker-list localhost:9093 --topic test
Hello world
I hope this makes it to the consumer

# Run the consumer to print all messages to standard out from the beginning 
$ kafka-console-consumer --zookeeper localhost:2181 --topic test --from-beginning
Hello world
I hope this makes it to the consumer

~~~

If all goes well we're good to start and create our first Kafka producer to bring in data from Twitter.

## Writing a Kafka Twitter Producer in Java

You can do this inside or outside the container. Either way we need to build a Java application. I used a [guide](http://saurzcode.in/2015/02/kafka-producer-using-twitter-stream/) by [saurzcode](https://twitter.com/saurzcode/) to write the Java Producer to connect to twitter. The code is available on the [saurzcode github](https://github.com/saurzcode/twitter-stream/).

Just make sure you change the metadata broker list property to point at port 9093 and feel free to change the hashtag of tweets to obtain.

~~~java

properties.put("metadata.broker.list", "localhost:9093");

...

endpoint.trackTerms(Lists.newArrayList("twitterapi", "#bigdata"));

~~~

The rest of the code will work fine - so simply package it into a JAR file with maven and if you built it outside of docker - transfer it to the docker container.

~~~bash

$ docker cp kafka-twitter-producer-1.0-SNAPSHOT.jar <container id>:/home/cloudera/Documents

~~~

Before you can run this make sure you have your twitter credentials obtained (the guide above explains how to do this) as you will need to pass them in as parameters in the next step.

Within the docker container execute the jar as follows:

~~~bash

$ java -jar kafka-twitter-producer-1.0-SNAPSHOT.jar <consumer key> <consumer secret> <token> <secret> &

~~~

That should start collecting tweets from the hashtag you entered into Kafka.

We can run the KafkaConsumer script from the quickstart example earlier to make sure our tweets are coming through

~~~bash

$ kafka-console-consumer --zookeeper localhost:2181 --topic twitter-topic --from-beginning

~~~

You should see some json objects being printed out to the screen containing tweet data for tweets that match your hashtag.

## Writing your own Spark Consumer

Now we've got our Kafka producer up and running, its time to write an application to consume and process these tweets. 

To do this we're going to build a [Spark streaming application](http://www.lewisgavin.co.uk/Spark-Streaming/). We will connect to our topic through the zookeeper instance (the same way the simple consumer above works) and generate ngrams from the tweet text. Every 60 seconds we will then list the top 10 3 word phrases from all the tweets collected in that time period.

1 - Set yourself a streaming context up and create a kafka stream

~~~scala


val scc = new SparkStreamingContext(conf, Seconds(60))

val kafkaStream = KafkaUtils.createStream(ssc, "localhost:2181", "camus", Map(("twitter-topic", 1)))

~~~

2 - Grab the topic content from the stream and set up a SQL Context

~~~scala
val lines = kafkaStream.map(x => x._2)

val sqlContext = new SQLContext(ssc.sparkContext)


~~~


3 - Within your stream, iterate over each RDD and parse out the tweet text (I used the [play framwork](https://www.playframework.com/documentation/2.0/api/scala/play/api/libs/json/package.html) to parse the twitter json object.

Build ngrams from the tweet words (after some cleaning up)

Do a word count on the ngrams to count the number of times each ngram has appeared.

Sort the RDD in descending order and print out the top 10 three word phrases for your hashtag in the last minute.

~~~scala

lines.foreachRDD{ rdd =>
  //extract the tweet text from each tweet
  val tweetText = rdd.map(tweet => (Json.parse(tweet).\("id").as[Int], Json.parse(tweet).\("text").as[String]
  .replaceAll("[\\s]+"," ") //get rid of all the spaces
  .replaceAll("https?|bigdata|BigData|Bigdata", "") //get rid of common words (my hashtag was bigdata)
  .split("\\W").toList)) //split into a list of words
  
  import sqlContext.implicits._

  val df = tweetText.toDF("id", "text")
  // build ngrams of size 3
  val ngram = new NGram().setInputCol("text").setOutputCol("ngrams").setN(3)
  val ngramDataFrame = ngram.transform(df)

  val ngrams = ngramDataFrame.map(frame => frame.getAs[Stream[String]]("ngrams").toList)
    .flatMap(x => x) //flat map into a list of each ngram
    .map(ngram => (ngram,1))//map into tuple with a count of 1 ready to be counted
    .reduceByKey((v1,v2) => v1 +v2) //count each time every ngram occurs
      .map(tuple => (tuple._2, tuple._1))// make sure the count is the on the key side of the tuple ready to be sorted
      .sortByKey(false) // sort in descending order
    .take(10)//grab the top 10 phrases and print them out
    .foreach(println)

}

ssc.start()
ssc.awaitTermination()


~~~


Thats it! We have successfully built a Kafka producer to ingest data from twitter and a Spark Kafka consumer to process and transform those tweets to give us some (semi) valuable information. Your output should look something like this:

~~~
(9,#DevOps #DataCenter RT)
(9,# #DevOps #DataCenter)
(3,| DevOpsSummit #APM)
(2,IoT Agility |)
(2,#IoT #IoE #)
(2,#DataCenter RT EdKwedar:)
(2,Agility | ThingsExpo)
(2,ThingsExpo #IoT #IoE)
(2,#DataCenter RT gpat1976:)
(2,DevOpsSummit #APM #DevOps)
~~~

## Wrap up

Once you have your stream coming in the possibilities are endless. Within the forEachRDD loop you are practically free to manipulate the tweets in whatever way you feel. We could also clean this up by adding a function to cleanse the tweets to remove any non alpha characters etc. so that we get a cleaner and more accurate output list - but I'll leave that to do as an extra.

Thats it for Part 2 - we have successfully ingested some data. Next time we will be looking at ways of storing our transformed data in a way that can be easily updated and then visualised. Make sure you check back next week for the next part of the series.