---
layout: post
title: Adding Google Analytics and Google AdSense to a Jekyll Website
author: Lewis Gavin
comments: true
tags:
- Git
- Jekyll
- Google
---



## Google Analytics

Google Analytics allows you to get information about your sites visitors such as the devices or OS they were using and their location. It goes without saying that information like this can be very useful for deciding how best to deliver content.

I have recently added Google Analytics to my Github site that is built using Jekyll, here's how I did it.


### 1. Sign up

Signing up for Google Analytics is quick an easy. Simply follow the steps here: [Google Analytics](https://www.google.com/analytics/web/?hl=en)

Make sure within the **Default URL** section you include your full URL, for git hosted websites that is USERNAME.github.io


### 2. Placing the code within your site

Once you are signed up, you will be provieded with some tracking code! 

Copy this code and I recommend that you paste it within the _layouts/default.html file, right towards the top within the **head** tag. This will ensure that the code snippet will be added to every page so you can keep track of your whole website.


### 3. Reporting

Now you are all set up, simply publish your website (note your site must be live for this to take effect). Then within the Google Analytics home page, select the **Reporting** tab at the top of the page. From here you will be able to see reports for the number of page views, geospatial information, technology and even your sites behaviour. 


## Google AdSense

Google AdSense allows you to publish advertisements on your website to help build revenue from your site.

Again, I recently added this to my Github site built with Jekyll.


### 1. Sign up

Quickly sign up for AdSense using the following link: [Google AdSense](https://www.google.co.uk/intl/en/adsense/start/)


### 2. Placing the code within your site

My Github hosted website is being used primarily as a blog... as you can see. So what I wanted to do was to host a small advertisement at the end of each blog post, just above the comments section.

To do this, take the provided code and create yourself a file called **advertisements.html** within the _includes folder. Within this file simply paste your code.

Then go into the **_layouts/posts.html** file (or any layout file for which you would like ads displayed) and insert the following code (without all the backslashes).

`\{\% include advertising.html \%\}`

I did this right towards the bottom, but the placements is totally up to you.

You should then recieve ads on all pages that utilise that layout!


## Linking with Analytics

You can link Analytics and Adsense together from the adense home page and clicking **Integrate with Google Analytics** and following the instructions. Couldn't be simpler!

_Note: If you want to see the ads by Google AdSense running on this page, remember to disable your adblocker on this page;)_

Hope you find this useful. Feel free to comment if you have any questions.
