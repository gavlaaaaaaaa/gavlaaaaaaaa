---
layout: post
title: Why you should move to an Event-Driven Architecture
author: Lewis Gavin
comments: true
tags:
- bigdata
- realtime
- eventdriven
- dataarchitecture
---

![real time event driven architecture](https://www.lewisgavin.co.uk/images/event-driven.jpeg)

Writing code is easy yet building software is hard. It doesn’t matter how good your code is, if the underlying architecture isn’t right you’ll still be left with bad software. Maybe not today. Maybe not a few months from now.

**But eventually, you’ll be left with bad software.**

In this post I’ll provide you with another tool for the toolkit that will help shape how you think about building software, to ensure it’s something you can be proud of in years to come.

## What is Event-Driven Architecture?

An Event-Driven Architecture is as its name suggests, a way of building software so that events drive everything.

It’s based on the notion that if everything is an event, we then build things that respond to those events and maybe generate further events of their own. It’s a fairly simple premise but can often be a paradigm shift from how we usually design software.

I can bet that most of the time when you think about building software, you think of things in a Request Driven way. Don’t worry, if you do you’re not alone, I do too. In the rest of this post though I’ll teach you how to think a different way that will offer you a new perspective when building software.

## Changing from Request Driven to Event-Driven

First of all, let’s define what the usual Request Driven world looks like. Let’s imagine a simple website that is made up of a User Interface (UI), an API and a Database. A fairly common pattern in the modern world.
When a user interacts with the website, let’s say they’re creating an account, they enter their details and click the sign-up button. This event triggers a request to the API. It requests that the API create a user with the submitted details.

![Simple Request Driven Architecture Diagram](https://www.lewisgavin.co.uk/images/request-arch.jpeg)

This event triggers the API to submit a request to the database to create a new record in the user table. The success or failure of the database request is then passed back up the chain to the API, who then passes this back to the UI which can then decide the appropriate action based on the response.

Notice how often in this scenario that it’s an event that is the trigger for the request to take place.

The way we think is often Event Driven by default. We just don’t realise it.

Now let’s look at making this website truly Event-Driven. To do this we need to remove the requests out of the scenario. Remember that the goal of an Event-Driven application is that the events drive as much as possible. If that’s the case then we can’t have a different part of the application making requests too, otherwise, they would also be driving things.

So let’s remove the requests from our scenario. A user fills in their account details on the website and clicks the sign-up button. Now we simply generate an event, lets call it a createUserRequested event.

![Event-Driven Architecture Diagram](https://www.lewisgavin.co.uk/images/event-arch.jpeg)

We then need something to react to that event, let’s call it the UserCreationService. This service reacts to this event by creating a user in the database. Once done it generates another event called createUserConfirmed.

The UI now reacts to the createUserConfirmed event and again takes the appropriate action and the cycle is complete.

The change is subtle but it’s an important one. Now our services no longer demand and request things from each other.

In fact, they don’t even need to know that the other exists at all.

Now you’ve built an application with components that are no longer tightly coupled. Each component goes about its business, raising events when an event happens and then listening and reacting to other appropriate events.

Let’s take a look at how this subtle difference can have a huge impact.

## Benefits of Event vs Request Driven

### 1. Decoupling Services

The first and hopefully most obvious benefit that I just touched on briefly is that the components of the application are no longer coupled together. This decoupling first of all requires that logical components are physically separated.

This separation ultimately means each component is now easier to maintain and build upon going forward. The UI no longer cares about the API, it cares about the createUserConfirmed event that the API (or userCreationService) generates. It doesn’t care where that event comes from, meaning, you guessed it, it can come from anywhere.

You now have full flexibility to update, change, break out or even outsource your userCreationService. The UI doesn’t care, all it needs to do is tell the world that a user needs to be created and wait for someone to reply that it’s been done.

### 2. One Event, Multiple Listeners

Now we’re into the interesting part. This, in my opinion, is the most important benefit and one that will unlock the most potential.
Since the services are declaring that something has happened (like a user requests to be created) instead of stating what should happen, any other service that cares about what has happened can also respond to the event.

Let’s go back to the user creation example. The UI raises a createUserRequested event and as before the UserCreationService handles creating the user and raises a createUserConfirmed event.
The UI can listen out for this confirmation event but so can any other service. Let’s say you want to send a welcome email to new users. You create a WelcomeEmailService that also waits for the createUserConfirmed event and when detected, sends an email to the new user.

![Event-Driven with multiple services](https://www.lewisgavin.co.uk/images/event-services.jpeg)

We could also have a DataWarehouseService that copies the new user to the data warehouse, and a CRMService that sends the user data to the CRM tool. All of these things can happen simultaneously, totally independently of each other, yet triggered by a single event.

We’ve now successfully moved away from having a god-like API. One service that used to know how to send welcome emails and send data to the data warehouse or CRM tool. It was therefore tightly coupled as it had to know how to make requests to all of these services, so any change in one could break the other.

What we’re left with is a loosely coupled system, where any service can be upgraded or swapped out without any impact on any other service.

Now, this might look to you like more services to maintain instead of just the one. I can assure you as a software engineer that I know which scenario I’d prefer and it’s the multiple smaller services option.

It’s so easy to jump into a codebase that’s responsible for a single thing and make changes knowing you’re not going to break other services.

I can add new services whenever I want something extra to happen after a specific event.

## Technology Example

Up to this point, this post has been fairly conceptual. In the final part, I’ll talk through a common technology choice for implementing an Event-Driven architecture.

### Apache Kafka
I’ve written about Apache Kafka before in my post [“What is Real-Time Data?”](https://medium.com/@lewisdgavin/what-is-real-time-data-37331ff91704) which is a great place to start if you have no prior knowledge of Apache Kafka.

It’s a highly recommended tool for event-driven applications mainly due to its reliability, flexibility and scalability (all of which I discuss in the above post).

The basic premise of Kafka is that you can send messages via queues (called Topics). Kafka deals with all the complexity that comes with managing queues, ensuring high throughput of data etc. All you need to do is create Producers that add messages (events in our example) to a Topic and Consumers that read these messages.

So let’s see how you’d implement it.

Going back to the user creation example. The UI service would act as a Kafka Producer and create userCreationRequested events on a Kafka Topic. You could have a single topic dedicated to these events, let’s call it user-creation-requested.

The UserCreationService would first of all be a Consumer that listens to the user-creation-requested topic, and for each new message would attempt to create a user with their submitted details. On success it would then also be a Producer that adds UserCreationCompleted events to a separate topic, let’s call that user-creation-completed.

Our UI service would now also act as a Consumer and be listening to the user-creation-completed topic, awaiting confirmation that the user has been created.

Our WelcomeEmailService would also be a Consumer, listening to the user-creation-completed topic and for each new message, simply send a welcome email to the user.

Where Kafka’s separation of producers and consumers comes in really handy is when services fail.

![Broken services mean request-driven breaks down](https://www.lewisgavin.co.uk/images/broken-service.jpeg)

Imagine that the WelcomeEmailService is down and we’re using the Request Driven architecture. The API would have tried to send the welcome email, failed because the service was down, then what? Log an error and continue maybe? Retry once or twice possibly? Either way, the user wouldn’t have received their email.

Back in the new and improved Event-Driven Architecture, Kafka is great for dealing with this problem. Each Consumer is in control of what they consume and how fast they consume it. Kafka simply stores an ordered list of the messages and each consumer reads each message one by one and remembers their place in the queue. This means that even if the WelcomeEmailService was down for an hour, once restarted it would simply pick up where it left off from, sending all outstanding emails to users that were created in the last hour.

![Broken services using Kafka can pick up where they left off](https://www.lewisgavin.co.uk/images/broken-service-kafka.jpeg)

No other services are affected, they don’t have to care that the WelcomeEmailService was down, they can continue consuming the messages at their normal rate. The WelcomeEmailService can catch up, every user gets their welcome email and we all lived happily ever after.

—

Hopefully, this sparks a different way of thinking when it comes to designing your next application. The benefits of Event-Driven, in my opinion, heavily outweigh those of a Request Driven and once you get the bug, I’m sure you’ll never look back.