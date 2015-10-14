---
layout: post
title:  Domain Driven Design for Smart Client Applications
date:   2015-03-08 9:11
category: eventsourcing
tags: software smartclients ddd angular
---
Attending [ng-conf](http://www.ng-conf.org/), I arrived with a negative attitude about technologies like [Firebase](https://www.firebase.com/) and [Meteor](https://www.meteor.com/) - what I would describe as back end application platforms, kitchen sinks that would otherwise replace all or most of the services of a proprietary backend.  This is neat stuff, and I've used [Parse](https://www.parse.com/) before, for a mobile app, and was pleased to do so.  The thing is that I didn't see how these tools mattered to me, and other developers, building complex applications.  I couldn't see how I could leverage these new architectures to solve non-trivial business problems.

The attitude comes from imagining that the popularity is mostly unfounded since many business problems are trivial at first and so the rapid progress achieved with a tool like Firebase is exciting - "with only a few lines of code" you have a mature backend and [dancing unicorns](https://www.youtube.com/watch?v=RW6Lp3Y3Vxs).  Some problems stay trivial and that's fine.  Many don't and then what?  Will Firebase be a welcome partner as you journey into complexity?  I just don't think so.  That's my hunch.

These backend-as-a-service companies were everywhere at the conference:  [Firebase](https://www.firebase.com/), [Netflix's Falkor](http://youtu.be/WiO1f6h15c8?list=PLOETEcp3DkCoNnlhE-7fovYvqwVPrRiY7), [Back&](https://www.backand.com/), [wakanda](http://www.wakanda.org/) - it was hard to keep up.  Firebase's booth was _popular_; Jafar Husain's presentation about Falkor got everyone's attention.  While I haven't at all compared each of these services, the common feature that interests me is data persistence.

All of this fits into my exploration of browser-side event sourcing.  So while I'm skeptical about how platforms like Firebase fit into solving hard problems, it's really interesting that there's all of this momentum to empower traditional "clients" by replacing custom backend services with general platform services whilst capturing the entirety of an application's business concerns in the web application itself.

The topic I'm taking a long time to get to is, given the taking-on of end-to-end business concerns by web applications, will Domain-driven design and other domain modelling strategies, and the implementations, become the work of web developers?  In most cases, until very recently, I think that what was considered to be the "real" domain modeling - if done at all - would not have been of much interest to web developers, or "front-end" developers to poke at that thing again.  I imagine that for Web developers "front-end" domain models would emerge from design specifications, mock-ups and APIs.  And then a whole other domain model would be implemented on the other end of the stack.

If Domain-driven design is useful, then we'll need to be open minded to it being implemented in unconventional places, like in collaborating web applications deployed to browsers or other clients (like native applications).

In my exploration, [Event Store](https://geteventstore.com/) is taking the place of Firebase.  Besides the persistence mechanism, the two technologies may not really be that different.  Maybe instead of clunking around building a "Boards" application, I should start with a version of the Firebase "Hello World" using Event Store.