---
layout: post
title: How to fuzzy match in Redshift
author: Lewis Gavin
comments: true
tags:
- aws
- redshift
- bigdata
---

![Fuzzy Merging — Photo by [Markus Spiske](https://unsplash.com/@markusspiske?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)](https://www.lewisgavin.co.uk/images/fuzzymatch.jpg)
Fuzzy Merging — Photo by [Markus Spiske](https://unsplash.com/@markusspiske?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

If you’re lucky, when working with multiple datasets within your data warehouse there will be some kind of join column available for tables you want to bring together.

When this is the case, life is good. However modern _Big Data_ solutions are opening up the use case for bringing all sorts of data together from disparate sources.

Although we can [easily store this data in one place](https://medium.com/@lewisdgavin/how-to-architect-the-perfect-data-warehouse-b3af2e01342e), joining them for analysis isn’t always as simple, as these datasets often haven’t been produced by the same source system so clean ID columns to join on aren’t always available. Even columns that provide you with the same information don’t always do so in the same format — and if it’s user captured you can never guarantee consistency.

One way of joining datasets together based on similarity is Fuzzy Matching — especially when you have text based fields in each datasets you know are almost similar, e.g. a user entered company name or product name.

This post wont go into detail about all the details of fuzzy matching but will show you how to utilise a Python implementation within Redshift.

### Fuzzy Matching

At its simplest level, fuzzy matching looks to produce a similarity score of how similar two things are. I’ll focus on comparing strings to explain the concept.

As humans we can easily spot typing errors or conceptually understand when two similar things are the same. Fuzzy matching algorithms try to help a computer do this. Rather than a match between two strings being a boolean true or false i.e. exactly the same or not the same, fuzzy matching gives a closeness score.

Take the strings ‘brooklyn bridge’ and ‘brooklin bridge’. A human would easily be able to spot that these are the same thing even though there’s a spelling error in the second string.

A fuzzy matching algorithm such as [Levenshtein distance](https://en.wikipedia.org/wiki/Levenshtein_distance) that gives a percentage score of similarity would probably score these two strings as at least 90% similar. We can use this to set a threshold of what we want “similar” to be, i.e. any two strings with a fuzzy score over 80% is a match.

### Python Implementation

These days you can pretty much find a Python package for anything so I’m not going to reinvent the wheel. A decent python package that implements fuzzy matching using Levenshtein is [**fuzzy wuzzy**](https://github.com/seatgeek/fuzzywuzzy)**.**

Following the installation process stated in the docs you end up with a bunch of functions for comparing strings.

The one I’ll be using for this example is the simple ratio function that takes two strings and gives a closeness ratio. Here’s an example implementation showing the Brooklyn bridge example from earlier.

~~~python
from fuzzywuzzy import fuzz  
fuzz.ratio(“brooklyn bridge”, “brooklin bridge”)

> 93
~~~
As expected this returns a fairly high score as the two are very similar.

### Redshift UDF

User Defined Functions allow you to add repeatable code blocks to Redshift using either SQL or Python. The python support will allow us to take the implementation from the previous section and add to Redshift so we can simply call it like any other native SQL function.

First of all we need to add the fuzzywuzzy library to Redshift. There is some [thorough documentation](https://docs.aws.amazon.com/redshift/latest/dg/udf-python-language-support.html#udf-importing-custom-python-library-modules) but I’ll outline the basic steps below.

1.  Download the [fuzzywuzzy](https://github.com/seatgeek/fuzzywuzzy) repo from github
2.  Take a copy of the fuzzywuzzy folder within the repo and zip it up.
3.  Copy this zipped folder to an S3 bucket
4.  In Redshift run the following to import the fuzzywuzzy library

CREATE LIBRARY fuzzywuzzy LANGUAGE plpythonu FROM 's3://<bucket\_name>/fuzzywuzzy.zip' CREDENTIALS 'aws\_access\_key\_id=<access key id>;aws\_secret\_access\_key=<secret key>'

Once done we can now go ahead and create the function using this library in Redshift.

~~~sql
CREATE FUNCTION fuzzy\_test (string\_a TEXT,string\_b TEXT) RETURNS FLOAT IMMUTABLE  
AS  
$$  
  FROM fuzzywuzzy import fuzz   
  RETURN fuzz.ratio (string\_a,string\_b)   
$$ LANGUAGE plpythonu;
~~~

We can now test it and check we see the same results as we did locally.
~~~sql
SELECT fuzzy\_test('brooklyn bridge', 'brooklin bridge');

> 93
~~~
### Wrap Up

And it’s as simple as that. It’s a nice feature allowing Python UDF’s giving you a lot of flexibility due to the range of libraries available in Python.

Due to the power of the Redshift cluster this means large scale fuzzy matches are possible that would probably never finish on a laptop. However…

One thing to consider if you’re going to use this for joins is that it will obviously be slower than usual as there’s not much optimisation that can take place on the join. So if you’re matching large datasets of strings then prepare for a long wait :)
