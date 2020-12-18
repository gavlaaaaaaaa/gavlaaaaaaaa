---
layout: post
title: Tesla’s Full Self Driving Beta Has Changed the Game
author: Lewis Gavin
comments: true
tags:
- tesla
- ai
- fsd
---

![Tesla’s Full Self Driving Beta Has Changed the Game](https://www.lewisgavin.co.uk/images/tesla-fsd-beta.jpg)

Tesla’s beta version of their Full Self Driving (FSD) software has been released to a small number of people and it’s mind-blowing!

This release gives us a glimpse at the (FSD) rewrite that Tesla has been working on in the last year or so. The aim of this limited beta is to prove the FSD package is safe enough to roll out to a larger audience.

Just before this release, I wrote an article detailing what Tesla has been working on and [how this release was going to be a “quantum leap”.](https://medium.com/ai-in-plain-english/teslas-next-autonomous-driving-release-will-be-a-quantum-leap-bce415a72c85)

Honestly, I think they’ve lived up to the hype. Here’s everything exciting that we’ve seen in the beta so far and what it means for the future of autonomous driving.

## FSD Beta Delivers New Features

For a long time now, any Tesla equipped with Autopilot has been able to stay in its lane and monitor its speed based on traffic. This feature was mostly for highway driving but overtime was good enough that it could be used on city streets, with a few caveats.

These features were called Enhanced Autopilot and the Full Self Driving package unlocked automatic lane changes and exiting/merging on to highways along with most recently, understanding traffic lights.
Each of these progressions slowly moved us closer to the promised full city street driving, where the car could take you from point A to point B with little to no interventions. However, -Elon said that their existing approach was trapped in a “local maximum”](https://twitter.com/elonmusk/status/1294722951896395776) and therefore it looked unlikely to deliver the full feature set.

To overcome this, the team has re-written the core FSD architecture meaning multiple images from the front camera array can now be labelled and processed together and “correlated in time”. Before each image was processed independently which is why they were struggling to progress.

## So what does all this mean? What has it delivered?

Here’s a list of all the cool features that the FSD beta has unlocked and links to example footage from the various beta testers.

- Left and right turns on city streets
- [Choosing the correct lane at traffic lights to make turns/continue](https://www.youtube.com/watch?v=nY0cq_IXUaw)
- [Yielding to oncoming traffic in order to make turns](https://youtu.be/6DfZNjKuD_E?t=80)
- [Navigating roundabouts](https://www.youtube.com/watch?v=RxrIFSYi6n0)
- [Stopping and continuing at stop signs and even creeping to check for oncoming traffic if the corner is blind](https://twitter.com/kimpaquette/status/1321572359829135367?s=21)
- [Driving “over the lines” to avoid parked cars or cyclists](https://twitter.com/kimpaquette/status/1319739372665819137?s=21)
- Navigate around traffic cones
- [Driving on roads without lane markings](https://youtu.be/rJrZ_iUJDMA)
- [Slowing down for speed bumps](https://twitter.com/kimpaquette/status/1321201042856562689?s=21)
- [Full point A to point B (driver has to take over for parking)](https://twitter.com/i/broadcasts/1YqJDpgRBZOJV	)

One thing to note whilst watching these clips is that the UI is a “developer” interface. Its purpose is usually to show Tesla engineers what the car is seeing when developing and debugging the FSD software. For this beta, it has been enabled to the limited user set when using FSD on city streets to give them a better understanding of what the car is doing and seeing.

This is important as the car is still in beta so will not be perfect. Giving the beta testers some on-screen visualisations helps them understand what the car is seeing and hopefully prevent any surprise manoeuvers.

## The Future of Full Self Driving

As you watch some of the clips above, you’ll probably notice a few things. Firstly, the driving experience is by no means perfect and second, that drivers are still intervening to help the car out. But this is ok, in fact, it's what Tesla wants.

Tesla is using each of the beta testers as a driving teacher for their software.

They’ve been developing this for a while now, testing between themselves privately and Elon himself stated that he’s had an alpha version of this FSD rewrite for a while. The most important test for the software though is large scale, real-world use. No amount of simulation can equal the day to day situations of the real world, or as Elon would say ["reality is the hardest sim."](https://twitter.com/elonmusk/status/1322431151986925568)

Think back to when you were learning to drive. You start off with some theory and you’ve probably watched parents and others driving for years before getting in the driver's seat but none of this prepares you for actually driving on the road by yourself.

We learn by driving on real roads but with an experienced teacher who is there to guide us and intervene when we make mistakes. This is exactly the approach Tesla is taking with its FSD software. The FSD beta is a learner driver taking to the road for the first time and Tesla’s entire fleet of vehicle owners will be the teachers.

Already, in just a week, [the beta testers have received updates](https://twitter.com/brandonee916/status/1321437966657449986) that have vastly improved their experience and brought new features. This is why a gradual release of this beta is so important.

As more and more bugs are ironed out and this beta makes its way into more vehicles, not only in the US but worldwide, it’s only a matter of time before it is driving without its human teacher. How long this will take is still to be seen, however, Elon has said that if all goes well with the initial testing then a public beta should be released in the US by the end of the year.

## Telsa’s Generic Approach vs Waymo

The final huge thing to point out here is Tesla’s approach to full self-driving. You might wonder what’s taking Tesla so long when there are completely autonomous vehicles on the road today from companies like Waymo, which require no human in the driver's seat.

The reason Waymo can do this is that they use [highly detailed pre-built maps](https://letstalkselfdriving.com/about/how-self-driving-cars-think.html) that “highlight information such as curbs and sidewalks, lane markers, crosswalks, traffic lights, stop signs, and other road features.” This means they can only drive in areas that have been mapped but it gives them a detailed understanding of what the world looks like at the cars current GPS coordinates. They use cameras and lidar sensors to detect other cars, road signs and traffic light colours so the car can drive safely on public roads.

This approach gets you to “real” full self-driving a lot quicker. This is because it’s predictable, Waymo can guarantee that the car knows everything about its surroundings and therefore only has to prove it’s capable to drive in this location.

The downside is that it requires pre-built maps, meaning the car cannot drive itself in a location that has never been mapped and if an area changes (roadworks, roads are updated etc.), the maps need to be updated. The lidar sensors used in the car are also currently very expensive, meaning mass production costs could be too high. Commercial releases of these vehicles do not exist yet and therefore currently, Waymo vehicles are used as a Taxi service.

In contrast, Tesla’s approach is more “generic”. Tesla is effectively trying to solve human vision and decision making in the context of driving a vehicle. To keep costs low, they don’t use Lidar sensors (that are great at measuring the distance of objects) and instead hope to solve this problem the way humans do, with vision (using cameras). This keeps the cost of the vehicles low, as cameras are relatively cheap but the downside is the software takes longer to develop.

This approach means that Tesla vehicles are being built to drive anywhere and deal with any scenario that a human driver would encounter. They are effectively building a machine that sees and interprets the real world as a human would. This means their cars can and will be full self-driving almost anywhere on earth.

Complications arise as you enter into different countries with different road laws and signs, however, with a fleet of vehicles already collecting data in these countries, it’s just a matter of using this data to “teach” the vehicle to drive under these new restrictions. This is exactly the same thing that we do as humans when renting cars when on vacation in a different country.

...

Although Tesla’s approach currently requires humans at the wheel and isn’t fully autonomous, based on this new beta it’s only a matter of time before they get there. Once they do, it will unlock FSD for a lot of people in privately owned vehicles in many different locations, something that other approaches will struggle to deliver.

This generic solution is obviously taking longer to train but will ultimately provide the largest payoff, as Tesla Model 3 owner with FSD I hope the wait isn’t too long.
