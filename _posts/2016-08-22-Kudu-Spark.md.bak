--- 
layout: post 
title:  Real Time Updates in Hadoop with Kudu, Big Data Journey Part 3
author: Lewis Gavin 
comments: true 
tags: 
- docker 
- hadoop
- pentaho 
---

![Kudu-Spark-Part3](../images/kudu-spark.png)

You've made it to Part 3, congrats! In [part 1](http://www.lewisgavin.co.uk/CDH-Docker) we looked at installing a CDH quickstart docker container along with Pentaho BA. In [part 2](http://www.lewisgavin.co.uk/Data-Ingestion-Kafka-Spark) we created an ingestion pipeline using Kafka to read data from twitter and obtain a word count of the most popular phrases for a specific hashtag.

For part 3 we're going to look at ways of storing this data to be reported on: using Kudu for real time reporting and impala for our historical data.

## Installing Kudu

Every time we start a series it feels like we're installing stuff. However it's never a bad thing to use some new technologies and become familiar with our application stack.

To install Kudu go to the cloudera package page and update your yum repo to contain the Kudu package. Once complete simply run the following commands:

~~~bash
$ sudo yum install kudu
$ sudo yum install kudu-master
$ sudo yum install kudu-tserver
~~~

Next we need to **uninstall** impala - this is because we need to download the impala-kudu client instead in order to interact with data stored in Kudu. 

~~~bash
## Uninstall Impala - this should remove all dependencies including state-store, catalog, server and impala-shell
$ sudo yum remove impala
~~~

Obtain the correct yum packages for impala-kudu from cloduera then install with the following commands:

~~~bash
$ sudo yum install impala-kudu
$ sudo yum install impala-kudu-state-store
$ sudo yum install impala-kudu-catalog
$ sudo-yum install impala-kudu-server
~~~

Now we need to start all these services using `service <service_name> start` and replace service name with `kudu-master, kudu-tserver, impala-state-store, impala-catalog, impala-server` **Note** when starting impala we use the same service names as before(without kudu in them) however it will start the version of impala that is configured to work with kudu.

**Potential issues**: If Kudu doesn't start, I found it was becuase of ntdp not running or not running correctly. To restart it simply run `service ntpd restart` and try to start Kudu again.

To check you are all up and running you can start the `impala-shell` and Cloudera give you a nice sql statement you can execute that checks whether you are set to go with Impala and Kudu.

## Creating a Kudu table with Impala

At this point, if you haven't already and aren't familiar with Kudu, it might be worth checking out my initial [beginners guide to Kudu](http://www.lewisgavin.co.uk/Apache-Kudu). Once you know the basics we can then crack on to creating our first Kudu table.

Start the `impala-shell`. We are going to create a table to store our ngrams and their current count. As stated in the Kudu post linked above, Kudu requires a unique key on every table that acts as the index. Seeing as we only ever want to store each ngram once along with its most current count, we can use our ngram as the key and the count as the value. Our schema will look as follows:

~~~sql
create TABLE `twitter_ngram` (
`ngram` STRING,
`count` INT
)
TBLPROPERTIES(
  'storage_handler' = 'com.cloudera.kudu.hive.KuduStorageHandler',
  'kudu.table_name' = 'twitter_ngram',
  'kudu.master_addresses' = 'localhost:7051',
  'kudu.key_columns' = 'ngram',
  'kudu.num_tablet_replicas' = '1'
)
~~~

As you can see, the syntax is as expected however we just need to define a few `TBLPROPERTIES` outlining that we want to store our data using the `KuduStorageHandler`, our kudu table name is `twitter_ngram`, our kudu master is running on `localhost:7051`(default), our key column is our `ngram` column  and importantly for us (seeing as we haven't configured tablet replication) is that our number of replicas currently `1`.

To check that our table exists in Kudu we can check through the Kudu UI through our browser. First we will need to find the ip address of our docker container (within your host machine terminal run `ifconfig`), and the port will be 8051 by default. Once you can see the UI click the *Tables* tab and you should be able to see our table.

Within Impala we can now try inserting and updating some values to get an idea of the syntax and how Kudu works.

~~~sql
-- Insert a test row into our table
impala> insert into twitter_ngram values ("big data", 3);

-- Take a look at our first row of kudu data
impala> select * from twitter_ngram;

-- Update the count to be 5
impala> update twitter_ngram set count = 5 where ngram ="big data";

-- Check it has been updated
impala> select * from twitter_ngram;

~~~

Now we're set to configure our Spark Application to start writing new grams to our table and updating existing ones.

## Using Spark with Apache Kudu

If we now return to our Spark Consumer application, we can build in our integration to Apache Kudu to start writing our ngram count data. 

The [Kudu developer docs](https://kudu.apache.org/docs/developing.html) give examples of how to integrate Kudu into a number of different technologies, including Apache Spark.

You will need some dependencies adding to your pom (if you're building with Maven).

~~~html
<dependency>
    <groupId>org.apache.kudu</groupId>
    <artifactId>kudu-spark_2.10</artifactId>
    <version>0.10.0</version>
</dependency>
<dependency>
    <groupId>org.apache.kudu</groupId>
    <artifactId>kudu-client</artifactId>
    <version>0.10.0</version>
</dependency>

~~~

You should then be able to import the necessary Kudu features to use within your application.

~~~scala
import org.apache.kudu.spark.kudu._
~~~

Now you have access to a bunch of different Kudu functions. Most notibly for us, is the `KuduContext`. This will allow us connect to our Kudu master so we can then write some data. To write data we simply use the `upsertRows` function thats made available by the context.

~~~scala
//Insert our ngrams data frame into the twitter_ngram kudu table but do an update if the ngram already exists
kuduContext.upsertRows(ngrams, "twitter_ngram")
~~~

We now have data flowing into our Kudu tables thats being constantly updated to reflect the last minutes worth of data. We could enhance this to keep track of the count and continue incrementing it so we can watch the NGram size grow over time.

## Storing Historical Data

The constantly updating data store is going to be really useful to give us the most current picture, especially when we start to build a real-time dashboard. However, in a few months time, if we ever want to find the most popular phrase was at a specific time a week ago, we aren't going to have that data anymore.

Storing the historical data can be really useful as we can run large batch processing applications to scan and spot trends over time or even use machine learning to predict what will happen in the future.

To store our history we are basically going to use the `HiveContext` to write our data out to hive at the same time as we do our upsert into Kudu. The only thing we need to add extra to our hive table is a timestamp so we know when the data was written. Your hive schema should look as follows:

~~~sql
create table historical_twitter_ngram (
    ngram_timestamp STRING,
    ngram, STRING
    count, INT)
stored as parquet;
~~~

The storage format for this Hive table can be whatever you please. Avro will give us the best compression and dynamic schema capabilities. Parquet will store our data in columnar format for quicker column based analytics. It all depends on your use case.

Our Spark code will then need updating to take our dataframe and using the `hiveContext` write the dataframe to hive too, but not forgetting to include the timestamp.

## Wrap up

We've come a long way - we now have a fully industrialised data stream being ingested into a hadoop cluster, that in real time calculates the most popular phrases being used for a specific twitter hashtag every minute. Data is being maintained and updated constantly and we're also storing the historical data for larger scale analytics at a later point.

In the next part of the series we're going to look at visualising our data and producing a dashboard.