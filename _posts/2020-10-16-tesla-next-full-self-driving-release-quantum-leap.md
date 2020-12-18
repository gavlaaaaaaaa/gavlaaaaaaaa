---
layout: post
title: Tesla's Next Full Self Driving Release Will Be a Quantum Leap
author: Lewis Gavin
comments: true
tags:
- tesla
- ai
- fsd
---

![Tesla's Next Full Self Driving Release Will Be a Quantum Leap](https://www.lewisgavin.co.uk/images/tesla-fsd.jpeg)

I’ve had my Tesla Model 3 for almost a year now and I purchased it with the Full Self-Driving (FSD) optional extra. At the time, this optional extra provided some self-driving features, but the car was far from driving me from A to B without human intervention. What I essentially bought was the promise of FSD in the future.

I, along with many others, believe that promise will soon be delivered and not just because Elon said so.

The new FSD re-write will soon begin its beta release. A few “safe” drivers who are part of Tesla’s Early Access Program will be getting this “quantum leap” re-write next week. Just saying that it’s a quantum leap doesn’t make it so.

However, the Tesla team have been working hard to overcome the current limitations. This time, they think they’ve got it right and I believe them, what I’m about to show you will clear up why. It’s a night and day difference.

## Full Self Driving Today

I recently wrote a post on [the capabilities of FSD (in the UK)](https://medium.com/swlh/the-truth-about-tesla-and-full-self-driving-in-the-uk-9a2ab4c0268c) and it’s current limitations.

Most of the current limitations are when driving on city streets. Tesla has done an amazing job at building an autonomous highway/motorway driving system. The car feels safe and confident driving itself even at high motorway speeds. However, engaging autopilot on a city street will give you mixed results.

If you’re just driving down a normal two-way road and don’t need to turn off down another street or navigate a roundabout, it’s fine. The car can follow the road, respond to traffic lights and adjust its speed based on speed limit signs or other cars around it.

When you reach a junction and need to make a turn, or navigate a roundabout to take the right exit, you currently have to disengage Autopilot and complete the manoeuvre yourself. If you come across parked cars on the side of the road or a cyclist, you have to disengage Autopilot and go around them yourself. Kind of annoying…

The current system can’t do these things because it lacks the context to do it safely. This is because currently Tesla’s self-driving AI uses images from the camera and effectively processes them in 2D space. Each frame from the cameras is given to the FSD computer which makes predictions and calculations about the space around it, but in two dimensions that's used to predict the 3D space.

Therefore, when you approach a roundabout or a junction and need to make a turn, you have a limited map of the world in front of you to make your predictions. This makes it very difficult to judge the layout of the road, what the corner looks like and what cars are doing to your left or right.

The following images are taken from a video where Andrej Kaparthy from the Tesla machine learning team talks about AI for fully self-driving cars.

![Tesla camera images of a junction (from Andrej Kaparthy)](https://www.lewisgavin.co.uk/images/tesla-camera.png)

These above images are taken from a Tesla vehicle as it approaches a junction. The images are from 3 separate cameras on the vehicle and have been put together to show how they are stitched together and processed by the FSD computer. For Tesla to deliver “full” self-driving, the car needs to understand how to navigate this junction and make a turn.

As you can see, it’s hard to understand as a human from these images alone what this junction actually looks like, we use context and previous experience to know that our car is on a vertical path and there is another car passing horizontally through our path on a joining road. If you were to map this road from a birdseye view this is what it would look like (the blue dot is the car).

![Actual birdseye view of the junction (from Andrej Kaparthy)](https://www.lewisgavin.co.uk/images/birds-eye-view.png)

When looking at the road like this, instead of from the plane of the car’s cameras, it’s a lot easier to understand how to make a turn either left or right, as you have a good understanding of the road layout.
The current FSD system, when trying to predict this birdseye view using these images and its current software comes up with something like this.

![Current Tesla FSD birdseye view prediction (from Andrej Kaparthy)](https://www.lewisgavin.co.uk/images/current-bev.png)

The red lines in this image indicate areas where there are obstacles and therefore the car shouldn’t drive. The lines curving left and right at the bottom are the curbs going around the corner. The tightly packed red lines in the middle are probably the trees in the background and the car passing by.

As you can see from the image, it understands the road branches out to the left and the right and that there is some kind of blockage ahead, but it’s kind of noisy. Making a right turn might be possible if we follow the red line curving to the right, but a left turn would be almost impossible without driving on the wrong side of the road.

However, even the right turn would be risky as we have no context that this is actually a junction and another road is passing in front of us, how would the car “lookout” for other cars travelling across from the left.

This is the current limitation with the existing full self-driving system and this is what the re-write aims to fix. When Andrej showed how the re-write predicts this junction, this was when I fully believed that the new system would be the “quantum leap” that Elon has just claimed.

## FSD Re-Write Introduces Birdseye View and “3D” Space

The Tesla team have been working hard to overcome these limitations so they can finally deliver the FSD package that has been promised to customers for over 3 years now. Here’s what they’ve come up with.

![From Left to Right: Actual birdseye view, new re-write prediction, existing FSD prediction (from Andrej Kaparthy)](https://www.lewisgavin.co.uk/images/actual-bev.png)

The image above shows you the side by side comparison of the new and existing software against the real-life example. That middle image is the birdseye view as projected by the new FSD re-write. Look at how close it is to the actual! It really is a night and day difference, no wonder Elon is calling it a “quantum leap”.

![Tesla camera images of a junction (from Andrej Kaparthy)](https://www.lewisgavin.co.uk/images/tesla-camera.png)

First of all, I think it’s incredible that using those same set of images (above for reference) Tesla can predict a birdseye view of the junction so close to the actual, considering all the cameras are viewing the world from the ground.

Modern machine learning techniques are incredibly powerful given enough data and this project showcases that at the cutting edge.

Think of how much easier it is for the team to build autonomous software that can navigate left or right turns on that junction with the new birdseye view. Before, they only really knew the curbs veered left and right but had no idea about lanes, what the road coming across horizontally looked liked or that there was the option to continue straight.

![new re-write prediction (from Andrej Kaparthy)](https://www.lewisgavin.co.uk/images/new-rewrite-prediction.png)

This is why this re-write is so important and will allow the Tesla FSD package to become “feature complete”. Having a much clearer view of the “3D” space around allows the car to make safer and more consistent decisions when it comes to navigating the world.

Projecting other vehicles onto this “3D” map will allow for better depth understanding too. This is important as to make the right turn on this road we want to make sure no cars are coming in from our left that we could collide with. Having this birdseye “Grand Theft Auto” style map gives the car a lot more context not only about the road but about the location of other vehicles.

I’m excited to see how the car now evolves once this re-write comes to more cars in the next year or so and starts allowing proper, no human intervention, A to B autonomous driving in a general capacity.
I know I’ll feel safer using this functionality knowing my car sees this new birdseye view of the world instead of the noisy and unintelligible view it has at the minute.

What do you think about this re-write, do you think Tesla has finally nailed it? I’d love to hear your thoughts below.

