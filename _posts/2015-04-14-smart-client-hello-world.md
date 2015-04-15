---
layout: post
title:  "Smart Client Journey - Hello World"
date:   2015-04-14 07:30
tags: software smartclients eventsourcing
---
A few days ago, more than two months since I mentioned the idea in my last post, I finished a rudimentary "Hello World" application which demonstrates in a basic way what it looks like to build a real-time, client-side application using event sourcing.

You can play around with the [demo in Azure](http://esjs-journey-1.azurewebsites.net/app/index.html) and [see the source](https://github.com/jskrepnek/esjs-journey-1).  I spent no small amount of time deploying Event Store to Azure, mostly the result of sparse documentation and extensive ignorance about Windows networking.

For better or worse, Event Store doesn't come with an "installer".  As I lumbered through the process of setting up Event Store I thought more than once that I should create an installer, to spare the next poor ignorant person from the same trauma.  I decided afterwards that since I learned a lot I wouldn't be doing anyone a favour by making it easier.  With that said, see this [post](https://groups.google.com/forum/#!topic/event-store/SBgFKYLdGaw) and this [repository](https://github.com/openAgile/EventStore-DevOps/tree/master/azure-powershell-windows) for a set of great PS scripts for automating an Event Store installation in Azure.

 The application is a simple message board that users can post to.  Any connected users will see each others messages when posted.  Messages that were posted before a user connects are not retrieved: the stream is processed "forward" from its current position - I say forward, but that's not quite right because you end up traversing the "previous" links to get new events.

Event Store's Http interface is based on the [AtomPub protocol](http://bitworking.org/projects/atom/rfc5023.html).  This is distracting at first, especially for a trivial application which won't benefit much the protocol's cleverness.  If you've never worked with AtomPub before, like I hadn't, it's a bit of a doozy.  You end up needing to simultaneously figure out it and Event Store to make progress.  Makes for a steep learning curve.  Notwithstanding that the author of AtomPub [half-jokingly describes it as a "failure"](http://bitworking.org/news/425/atompub-is-a-failure), you can appreciate the brilliant insight that the Event Store guys had by marrying the AtomPub protocol to the store's native Http interface.

I implemented the ability to subscribe to a stream and append an event to a stream.  The [type of subscription](http://docs.geteventstore.com/introduction/subscriptions/) was initially built to catch-up from the beginning of the stream, but this wasn't ideal after I had posted a lot of messages.  From there I switched to a volatile subscription, which will only get events that are written after the subscription is established.  Infinite scroll could be realized by paging "backwards" (or, really, forwards! argh) through the stream.

The "realtime" effect is implemented through polling of the current event's "previous" link.  If the link exists then a more recent event exists and we recursively process the Uri.  Otherwise, we recursively process the same link again after a timeout.  Here's the code for that:

{% highlight js %}
processPrevUri = function (prevUri, onEvent) {
    $http
        .get(prevUri, {
            headers: {
                'Content-Type': 'vnd.eventstore.atom+json',
                'Authorization': 'Basic ' + token
            }
        })
        .success(function (data) {
            processEntries(data.entries, onEvent)
                .then(function () {
                    var previousLink = getLink(data.links, "previous");

                    if (previousLink) {
                        processPrevUri(previousLink.uri, onEvent);
                    } else {
                        $timeout(function () {
                            processPrevUri(prevUri, onEvent);
                        }, 500);
                    }
                });
        });
};
{% endhighlight %}

I'm going to go out on a limb here and say that I suspect that this kind of polling by many connected clients would not scale.  Again, just surmising here, a better subscription model would probably combine a server-side observer that is pushing to clients using sockets or something cooler than polling.  On the other hand, this is simple and that's worth something.

What's next.  I think the next journey is to explore collaborating over multiple streams.  To pull this off, each client needs to be able to discover all of the message boards that have been created.  This is harder than it sounds if you have the questionable goal, as I do, of avoiding any server-side code.  No two message boards know about each other, but somehow we need to assemble a list of boards.  We can use a [projection](https://geteventstore.com/blog/20130212/projections-1-theory/) to accomplish this, which might create a new stream that aggregates message boards, or which might just keep a list as state that client's can query.  But as I say that's cheating.

If no security model is necessary then clients could subscribe to the $all stream, but what useful application doesn't have a security model?  Off the top of my head the only other option I see is to have clients themselves post events to a reliably discoverable stream called, say, messageBoards.  This still doesn't propose a security model, but it's less incompatible so let's start there.  That decided, I can already tell that I'm more interested in proofing out a private message board.  I think that's going to be the important problem to explore.