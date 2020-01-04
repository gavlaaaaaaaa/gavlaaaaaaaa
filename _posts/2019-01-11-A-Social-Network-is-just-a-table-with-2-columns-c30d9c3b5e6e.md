---
layout: post
title: A Social Network is just a table with 2 columns
author: Lewis Gavin
comments: true
tags:
- dataengineer
- datawarehouse
- socialnetwork
---

… but my sentiment still stands. The essence of a social network can easily be stored in a single table with two columns.

This post was inspired by the complex way social networks are depicted, especially in diagrams and it made me think about how to simplify the idea of it so that anyone could understand it.

### Let’s start by outlining what a Social Network is

In simple terms it’s a network that describes social relationships and interactions. They existed _waaaaayyyyy_ before the internet. Cities, Towns, Villages and people have all had their own little social networks for as long as people have existed, in fact as long as any social living organisms have existed.

The term however, blew up once the internet allowed Social Networks to be documented, stored and expanded.

These days, your social network can include more than just people you know and interact with on a regular basis. It can include people you have never even met before or those met solely through an online exchange.

**Kind of crazy when you think about it.**

But none of this is new. We’re all well aware of this, but what do these online social networks actually do.

![Photo by [Dương Hữu](https://unsplash.com/@huuduong?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)](../images/socialnetworktable.jpg)
Photo by [Dương Hữu](https://unsplash.com/@huuduong?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

> They store a link between one person (or entity if you’re being really pedantic) and another.

That’s it. **Just a simple link between two people**. Lots and lots and lots and lots of times.

This scale is why I think we picture them as being complex things but if you take it back to the core principal: storing a link between two people, it doesn’t seem too bad now right?

### But you said it was a table with two columns, please explain…

… well if a social network is just a link between two people stored on a huge scale. Then you need a way to store a link between two people. One way to do this is create a table that stores two user identifiers representing each person.

~~~sql
CREATE TABLE friend   
(  
   user_id VARCHAR(64),   
   friend_user_id VARCHAR(64)  
);
~~~
Done!

Well, not quite, but you get the picture. This table can now easily store a relationship (that we’ve called friend) between two users.

If you’re wondering about the order in which the relationship should be stored. Well if I was designing this I’d put the person that initiated the friend request in the first column and the person they requested in the second.

### What about graph traversal to find friends of friends?

Now we’re talking. Now we’re getting to those super complicated pictures of social networks designed to blow your mind.

![A super complex social network: [Image Source](https://thenextweb.com/socialmedia/2013/11/24/facebook-grandparents-need-next-gen-social-network/)](https://cdn-images-1.medium.com/max/800/0*PhHvBjrAWUOw22EN.jpg)
A super complex social network: [Image Source](https://thenextweb.com/socialmedia/2013/11/24/facebook-grandparents-need-next-gen-social-network/)

Let’s start off simple.

For a user, find their friends and then find all their friends, that aren’t already friends with the initial user.

This is how all social networks start off when showing you “Recommended friends” or “people you may know”. The logic is that if you know one person, theres a likelihood you may also know the same people.
~~~sql
SELECT user.user_id, friends.friend_user_id  
FROM friend user  
LEFT JOIN friend friends ON user.friend_user_id = friends.user_id  
LEFT JOIN friend same_friends ON same_friends.user_id = friends.friend_user_id   
WHERE a.user_id = 1 AND c.user_id IS NULL;
~~~
Again not too difficult. Although I’m not going to pretend that it wouldn’t get kind of unwieldy when you want to know friends of friends of friends and so on.

Funnily enough, Facebook came across the same problem and decided to go down the Graph database route. Graph databases are designed to help solve this sort of problem as they are built (obviously) with graph traversal in mind. This makes asking more complicated questions about a network more trivial.

### So is a social network really just a table with two columns?

Yes and no…

As you can see, the act of storing relationships between users is possible with a single table and two column. You can even query that data and use it to find recommended friends to help users build their network.

In reality though you’re going to want a lot of extra data around it to make it more usable in a real world scenario.

First of all you’re going to need tables to store data about users and their profile.

You’ll also probably want to have the concept of a friend request so you can differentiate between requests and actual friendships.

You’ll probably want to store metadata around the events of when a friend request was made and accepted.

Then expand to all the other _stuff_ that make up social networks including posts and likes — again likes are just relationships between a user and another entity, more than likely a user and a post.

**Hopefully this simplified look on the world of social networks has sparked some thought into how simple ideas can very quickly turn into some of the biggest and most powerful tools.**
