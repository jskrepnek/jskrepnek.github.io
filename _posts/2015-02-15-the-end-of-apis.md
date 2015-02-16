---
layout: post
title:  "The End of APIs"
date:   2015-02-15 14:39
categories: software smartclients eventsourcing
---
I've spent the better part of the last 3 years developing [full stack](http://www.laurencegellert.com/2012/08/what-is-a-full-stack-developer/) solutions that integrate rich but subservient "smart" clients with omniscient backend services realized through [Domain Driven Design](http://amzn.com/0321125215) and [Event Sourcing](https://msdn.microsoft.com/en-us/library/jj554200.aspx).

In practice I've used the excellent [NEventStore](http://neventstore.org/) as a persistence library.  But it was through experiments with [Event Store](https://geteventstore.com/), which is persistence infrastructure for events with a *native http interface*, that got me wondering: what would it look like to build a solution where a group of omiscient *clients* collaborate in real-time with events using Event Store.  Events _become_ the API.  Whatever that means.

Of course, there are obvious complications, like security, versioning, and concurrency.  None of these are trivial.

On the other hand, there are some really interesting consequences like:

* reduced transformations between a user action and an event
* greatly simplified backend infrastructure
* unified development experience (the front-end/back-end goes away)
* leveraging the rich javascript development ecosystem and the progressively richer browser application platform
* give up the terrible marriage between REST and task and/or command based architectures

And so on.

It's amazing how entrenched the web developer vs web designer and the front-end vs backend mythologies are.  It goes on ...

<img width=500 src="http://blog.salsitasoft.com/content/images/2015/01/Front-End-Developers-3.png"/>

and on ...

<img width=500 src="http://osudb.weebly.com/uploads/2/1/5/0/21507750/6247844_orig.png" />

and on ...

<img width=500 src="http://www.commitstrip.com/wp-content/uploads/2014/08/Strips-front-end-vs-le-back-end-650-finalenglish.jpg" />

and on ...

<img width=500 src="http://rlv.zcache.com/web_developers_do_it_on_the_back_end_tshirt-r8f9b568de9ef4da5a7e47f48c385721f_8nhmi_512.jpg" />

Maybe [there's other ways](https://www.firebase.com/).  For my part, I'll do some exploring with a rich web application and Event Store.



