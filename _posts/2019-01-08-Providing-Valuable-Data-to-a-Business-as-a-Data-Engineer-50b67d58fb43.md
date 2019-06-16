---
layout: post
title: Providing Valuable Data to a Business as a Data Engineer
author: Lewis Gavin
comments: true
tags:
- dataengineer
- datawarehouse
- bigdata
---


It can be easy to overlook the complexities of being a data engineer. After all we’re just moving data from one place to another right: how hard can it be?

There are certain skills however that I’ve found have helped me during my time as a Data Engineer to provide a valuable data asset to the companies I’ve worked for.

Often we’re tasked with taking a business requirement or question and finding a way to obtain, transform and join data to either find the answer ourselves or allow analysts to do their magic.

As with most things, theres a quick and easy route that is fine for some questions. However, most of the time the preferred route is to future proof.

We want to obtain data so it can be analysed historically but also continually be analysed in the future.

We also want to ensure it is prepped, clean, easy to use and contains other relevant data to help further questions down the line.

This post isn’t going to concentrate on the technicalities of data engineering, instead talk more about how I approach obtaining data to help answer questions and be ready for the questions that follow.

### Domain Knowledge

If you’re new to a business or the question being asked is outside of a domain you’ve explored before, you’re going to need to fill that knowledge gap.

I’ve found that there is almost certainly someone within the business who has a good background on the domain. Even if they have no idea about how to get to the answer you are after they’ll provide you with at least one key asset: names and keywords for “_things_”.

![Find the SME’s : [Image Source](https://rodneywade.wordpress.com/2015/06/21/we-are-smes-if-you-please/)](https://cdn-images-1.medium.com/max/800/0*xwd3HMY9yJB3J0EX.jpg)
Find the SME’s : [Image Source](https://rodneywade.wordpress.com/2015/06/21/we-are-smes-if-you-please/)

Being able to talk about the different _things_ that make up a domain will help you with research, further questions and most importantly naming.

> Future proofing your shiny new data can only be achieved by correctly labelling the objects and attributes associated with them.

Correct is also subjective here, as it should be correct for your business. If your company calls people ‘Users’ then you should do the same, if they call them ‘Customers’ then you should too.

This makes finding, analysing and understanding information easier for the whole business and therefore makes the data asset you are creating more valuable.

This also leads you to understand what other metrics and dimensions are relatable to the domain. Leading nicely onto the next section…

### Future Proofing

Getting the domain knowledge down is the key first step and leads nicely onto the future proofing of the solution.

From experience, I know that providing data to help answer a question is only going to fuel more questions.

> This is great, this is what new information should do, inspire new paths of thought and get people excited and hungry to know more.

![Data hungry! — [Image source](http://www.duncanmalcolm.com/startup-data-analytics-metric-tools/)](https://cdn-images-1.medium.com/max/600/0*VwvEXGhSKqwoyxZ6.jpg)
Data hungry! — [Image source](http://www.duncanmalcolm.com/startup-data-analytics-metric-tools/)

And you want to be ready to pounce when those new questions arise. There’s nothing worst than sparking a discussion and getting ideas flowing and then having to wait whilst new data is dug out.

This is where some experience and anticipation of questions comes into play, but the domain knowledge gained earlier will help.

At this point I usually start thinking about why people are asking the questions they’re asking and think of other avenues they will want to explore once they have the answer. You can do this by asking one simple question.

_Why is the answer higher/lower/better/worst than I expected?_

Asking **_why,_** forces you to think about the kinds of dimensions you’d want to break the domain down by.

If it’s number of negative reviews per product you’re being asked to find data for you’ll probably want some dimensions such as time, product type, customer profiling — you might even go as far as text analytics to find common phrases in negative reviews.

This gives you an idea of the kinds of extra data and metadata that might be required to further enhance what’s required to get to a basic answer.

Most of the time, you get these dimensions for free as normally attributes for a specific thing are usually collected with and therefore stored with it too. To get to the nitty gritty answers though, you’ll often need to branch out to other data sources to supplement what you have.

![Have all the answers : [Image Source](http://memeshappen.com/meme/donald-trump/i-know-lots-of-data-the-best-data-82002)](https://cdn-images-1.medium.com/max/600/0*odE4Y5cyPfSXq3Vo.jpg)
Have all the answers : [Image Source](http://memeshappen.com/meme/donald-trump/i-know-lots-of-data-the-best-data-82002)

What’s important is you’ve at least thought about what’s next.

So when people come with further questions you’ll have all the data, the best data (or at least know where it is) to help answer them.