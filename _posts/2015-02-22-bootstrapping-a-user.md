---
layout: post
title:  "Clientside Eventsourcing: Bootstrapping a user"
date:   2015-02-22 16:38
tags: software smartclients eventsourcing
---
When a user signs in, we need to be able to determine what aggregates ("Boards") they have created or, in a collaborative model, have been shared with him.  These represent the event streams that the Read infrastructure would observe to construct and update read models.  Since streams are globally unique, the applications can't guess at them.  So there needs to be a convention whereby a client can discover this information.

The only option of course in this architecture would be a stream that *is* discoverable, and the key must be the username.  That is the only other unique, known information that we have for the user.  After a user signs in, we can use his username to consume a stream containing events which will lead us to other streams that are more interesting (the "Boards").

I can think of a few approaches.

Projections
-----------
We could write an EventStore projection that would watch for any events related to stream access - like BoardAdded or BoardShared - and emit those events into a stream keyed to the subject's username.  

Process Manager
---------------
