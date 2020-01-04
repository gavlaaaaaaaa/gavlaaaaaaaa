---
layout: post
title: Complex DynamoDB data into Redshift made easy
author: Lewis Gavin
comments: true
tags:
- dynamodb
- redshift
- bigdata
---



![Photo by [Rodion Kutsaev](https://unsplash.com/@frostroomhead?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)](https://cdn-images-1.medium.com/max/2560/0*3xv6CkvIlCM7S4lH)
Photo by [Rodion Kutsaev](https://unsplash.com/@frostroomhead?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

It’s fairly common practice to store JSON payloads in DynamoDB. You have a few data storage options when doing this: storing the whole payload as a string or storing it within a DynamoDB Map.

Each have their pros and cons which I’ll discuss below and one approach in particular affects how we consume this data in Redshift. I’ll also outline how to deal with this scenario.

### String vs DynamoDB Map

Storing it as a string means you are flexible when it comes to writing data to your DynamoDB table. Because you are simply storing a string, it doesn’t really matter if your JSON payload is valid JSON or if its structure is different. The drawbacks here are that you obviously, at some point, need to make sense of this data in order to use it. This usually leads to more complex logic downstream to process it. You also can’t use native DynamoDB functionality to query this data.

Storing as a Map is often the preferred option as it’s easier to work with and supports all the normal DynamoDB features. You also have a structured object thats easier to use and parse by an application downstream.

This second option, as the preferred option means that developers can quickly push payloads to DynamoDB but keep a bit of structure. However as a Data Engineer looking to ingest this data, it becomes more complex.

### Ingesting DynamoDB data into Redshift

If you want to ingest DynamoDB data into Redshift you have a few options.

*   The Redshift Copy command
*   Build a Data Pipeline that copies the data using an EMR job to S3
*   Export the DynamoDB data to a file using the AWS CLI and load the flat file into Redshift.
*   You also have the option of DynamoDB streams or a Kinesis firehouse but I’ll save those for a future article.

#### Redshift Copy Command

This is the simplest way to get data into Redshift from DynamoDB or S3. You simply need to create a table in Redshift that matches that of your DynamoDB table and simply run a Copy command that looks as follows:

copy favoritemovies   
from 'dynamodb://ProductCatalog'   
iam\_role <insert iam role>   
readratio 50;

Data from the ProductCatalog DynamoDB table will be copied into the Redshift table called favoritemovies. Simple!

However there are drawbacks with this method that will prevent you from loading complex data types.

> Only Amazon DynamoDB attributes with scalar STRING and NUMBER data types are supported. The Amazon DynamoDB BINARY and SET data types are not supported. If a COPY command tries to load an attribute with an unsupported data type, the command will fail.

So what happens if we want to take the preferred approach from section 1 and use a DynamoDB map to store our JSON payloads? The Map datatype isn’t supported and therefore can’t be loaded using this approach. Let’s look at some other options…

#### DynamoDB to S3 Export using Data Pipeline

This is a supported option with an out of the box Data Pipeline that has been preconfigured for you.

Simply go into Data Pipeline within the AWS console and create a new pipeline. You will see in the templates drop down that there is a template preconfigured to export DynamoDB data to S3.

![](https://cdn-images-1.medium.com/max/800/1*Yf6XjQyLoID_ED-6pYrtlQ.jp2)

What this does is create a pipeline that spins up an EMR cluster to run a Map Reduce job that processes data from DynamoDB and puts it into a file format to be dumped into S3. As shown in the diagram below.

![Datapipeline — Dynamo to S3: [Source](https://docs.amazonaws.cn/en_us/amazondynamodb/latest/developerguide/DynamoDBPipeline.html)](https://cdn-images-1.medium.com/max/800/1*RVxpvzTFs-fQmIyxETXEsA.jp2)
Datapipeline — Dynamo to S3: [Source](https://docs.amazonaws.cn/en_us/amazondynamodb/latest/developerguide/DynamoDBPipeline.html)

You can obviously extend this pipeline to then copy that S3 data into Redshift. Note that the export will be in JSON format so you may need to provide a JSON paths file to help with the load to Redshift.

As you can see, this option will allow any Dynamo column types to be exported to a flat file and ingested into Redshift however you like.

#### DynamoDB to Flat File using AWS CLI

This option is a little more involved and will require more upfront development. The benefit though is that it gives you a little more flexibility and means you can be truly “schema-less” in your DynamoDB approach.

The reason being is that this approach will dump the whole DynamoDB row on a single line and we’ll import it into Redshift. There are a bunch of different ways of getting the data into Redshift too.

To get the data out of Dynamo we will simply use the AWS CLI and run a DynamoDB scan.

~~~bash
aws dynamodb scan --table-name 'myDynamoTable' --filter-expressions "<my filter expressions>"
~~~
This will pull data from DynamoDB and using the `--filter-expressions` we can get deltas by only getting records newer than a certain date.

Now this will come out as a JSON file that more than likely won’t be fit to put straight into Redshift, so we’re going to need to flatten it a little.

We can do this by piping our output into JQ: an open source JSON processor.
~~~bash
aws dynamodb scan --table-name 'myDynamoTable' --filter-expressions "<my filter expressions>" | **jq -r -c '.Items\[\]' --sort-keys -**
~~~
What this is doing here is printing the raw JSON (`-r`) in a compressed format (`-c`) and taking each of the items `.Items[]` and putting the whole JSON object for each DynamoDB Item on a single row. The `--sort-keys` option ensures that the columns are always in the same order.

From here we have a whole host of options. You can obviously extend you JQ statement to parse out your columns and complex Map objects into a format ready for ingesting. You may even want to create multiple files if your JSON items contain nested arrays and you want to ingest them into a separate table.

The alternative is to create a single column table in your Redshift staging area and load the whole row into this column. You can then use the [Redshift JSON functions](https://docs.aws.amazon.com/redshift/latest/dg/json-functions.html) to parse the data into suitable structures downstream.

The downside to this option is that you’re limited by the max storage size of a Redshift VARCHAR column so if your JSON payloads are particularly large this may cause an issue.

### What do you think?

Hopefully this has given you some ideas of how to overcome some of the restrictions when copying data between DynamoDB and Redshift.

I’ve personally used each of these options depending on the scenario and they work well, especially when you’re looking to batch ingest from Dynamo.
