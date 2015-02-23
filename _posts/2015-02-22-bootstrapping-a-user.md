---
layout: post
title:  "Browserside Eventsourcing: Bootstrapping a user"
date:   2015-02-22 16:38
tags: software smartclients eventsourcing
---
When a user signs in, we need to be able to determine what aggregates ("Boards") they have created or, in a collaborative model, have been shared with him.  These represent the event streams that the Read infrastructure would observe to construct and update read models.  Since streams are globally unique, the applications can't guess at them.  So there needs to be a convention whereby a client can discover this information.

In a traditional web application, the client, on behalf of an authenticated user, would query for of all of the resources of a certain type; in this exploration, that would be a Boards resource.  The read model would be a collection of Boards, probably with identifier and name properties.  You would use the identifier to drill into the details of each board.

The only option of course in this architecture would be a stream of "Boards" available to the user that *is* discoverable, and the key must be the username.  That is the only other unique, known information that we have for the user.  After a user signs in, we can use his username to consume a stream containing events which will lead us to other streams that are more interesting (the "Boards").

I can think of a few approaches.

On the Server - Projections
-----------
We could write an EventStore projection that would watch for any events related to stream access - like BoardAdded or BoardShared - and emit those events into a stream keyed to the subject's username.  The big drawback is that this results in a concerning, if at first small, transfer of responsibility from the client to the database.  The goal here is to keep all of the application and business login running in the browser.

On the Client - Process Manager
---------------
We could associate User aggregates with the Board aggregates belonging to that user via a Process Manager.  This would result in new events raised by the User Aggregate - such as UserClaimAdded - which remember is keyed to the username.  On the read side, the read model worker would subscribe to or unsubscribe from streams based on the "Claims" discovered in the User stream.

Tricky Bits
-----------

### Username Changes
I would worry about creating a long-lived stream that was keyed to a property that can change.  If users were issued unique identifiers on creation then perhaps those instead could be used.  This is where you might consider one departure of responsibility to an Identity and Access server, which would issue unique identifiers for users.

### Collaboration
When Bob shares a board with Sally, how does Sally's instance of the application discover that?  That could result in a command to Sally's User aggregate - but of course Bob would not be permitted to load that.  It would make sense that a BoardShared event would be raised on the Board aggregate by Bob.  Somehow the necessary ACL for the stream would need to change.  And then somehow Sally's client would need to be informed of this.  It sure feels like collaboration _between_ users might necessecitate some server-side software: either projections, or a full blown Identity and Access domain.