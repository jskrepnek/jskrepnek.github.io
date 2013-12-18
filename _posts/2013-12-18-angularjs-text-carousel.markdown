---
layout: post
title:  "AngularJS text carousel directive"
date:   2013-12-18 7:11
categories: angularjs directives
---

A primitive, non-interactive text carousel directive for AngularJS (<a target="_blank" href="https://gist.github.com/ekeonit/8024016">gist</a>).  It requires jQuery for the fading animations, but you could use pure css 3 transitions to remove that dependency.

A nice extension would involve seeding the values with an array of strings on the scope.  The interface could be made to support either use case seamlessly.

See it in <a target="_blank" href="http://embed.plnkr.co/8ReWhq/preview">action</a> at Plunker.

<script src="https://gist.github.com/ekeonit/8024016.js"></script>