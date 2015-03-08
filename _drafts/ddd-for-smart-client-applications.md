---
layout: post
title:  Domain Driven Design for Smart Client Applications
date:   2015-03-08 9:11
tags: software smartclients ddd angular
---
Attending ng-conf, I arrived with a negative attitude about technologies like Firebase and Meteor - what I would describe as back end application platforms, kitchen sinks that would otherwise replace all or most of the services of a proprietary backend.  This is neat stuff, and I've used Parse before, for a mobile app, and was pleased to do so.  The thing is that I didn't see how these tools mattered to me, and other developers, building complex applications.  I couldn't see how I could leverage these new architectures to solve non-trivial business problems.

The attitude comes from imagining that the popularity is mostly unfounded since many business problems are trivial at first and so the rapid progress achieved with a tool like Firebase is exciting - "with only a few lines of code" you have a mature backend and dancing unicorns.  Some problems stay trivial and that's fine.  Many don't and then what?  Will Firebase be a welcome partner as you journey into complexity?  I just don't think so.  That's my hunch.

These backend-as-a-service companies were everywhere at the conference:  Firebase, Netflix's Falkor, Back&, wakanda - it was hard to keep up.  Firebase's booth was _popular_; Jafar Husain's presentation about Falkor got everyone's attention.  While I haven't at all compared each of these services, the common feature that interests me is data persistence.

All of this fits into my exploration of browser-side event sourcing.  So while I'm skeptical about platforms like Firebase, it's really interesting that there's all of this momentum to empower "clients"