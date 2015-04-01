---
title: The Future of TodaysMeet
author: James
layout: post
permalink: /the-future-of-todaysmeet-552/
bitly_url:
  - http://j0.is/14dZDR6
bitly_hash:
  - 14dZDR6
bitly_long_url:
  - http://coffeeonthekeyboard.com/the-future-of-todaysmeet-552/
categories:
  - Articles
tags:
  - comet
  - django
  - events
  - html5
  - memcached
  - node.js
  - python
  - redis
  - sockets
  - todaysmeet
  - webdev
---
*This is the second half of a two-part post. Start with [part 1][1].*

[TodaysMeet][2] is an interesting challenge because it has components that are absolutely real-time and should be built like a messaging system, not a CMS, and parts that aren&#8217;t real-time at all, and can totally be built like a CMS.

TodaysMeet has one loosely-coupled component, its [Twitter][3] integration. The site knows nothing about the daemon that executes Twitter searches and populates the database. If anything, I think it&#8217;s currently *too* loosely coupled, but it&#8217;s given me some insight into how to build the new system.

The guiding principles of this design are:

  * **DRY**. Only one part of the app should know about the canonical data store (MySQL for now).
  * **Loosely coupled**. A failure in one part of the service shouldn&#8217;t affect other parts.
  * **Real-time**, event-driven. Periodic polling is bad. Periodic Twitter searches are particularly so.
  * Use the **right tool** for the right job.

Obviously I&#8217;m a fan of [Django][4]. I could port TodaysMeet directly, using something like [Playdoh][5] to bootstrap the process, and get done relatively quickly. Django gets me a lot of stuff for free: a solid data store, users (something I want to add in a limited capacity, eventually), even a framework for building the crons and daemons necessary for running the site.

But Django is a lousy way to serve real-time content via long polling or [socket][6] connections.

I can easily serve real-time content via [Node.js][7]. I can do long polling or sockets. It&#8217;s also a great way to handle Twitter integration because I can just leave a connection to their [streaming API][8], and handle new tweets as soon as they happen instead of checking periodically.

But even with things like [Sequelize][9], MySQL connectivity through Node still feels awkward. (I think it will get better but I&#8217;m impatient.) And serving fairly static content is just weird.

So, [Towelie][10], do you want to go real-time with Node, or do you want to use Django?

I&#8217;ll use&#8230; both!

Read on for a more technical overview. And a picture!

<!--more-->

The core of this system is [Redis][11], acting as a message broker and light-weight data store.

[<img class="aligncenter size-full wp-image-555" title="TodaysMeetInfra" src="http://coffeeonthekeyboard.com/wp-content/uploads/2011/01/TodaysMeetInfra.png" alt="" width="580" height="379" />][12]

The three yellow blocks are event-driven daemons. [Rasputin][13] is a binding that allows Django to subscribe to events from the message broker and act on them. That means I don&#8217;t need to pass messages between Node and Django over HTTP. Rasputin also includes bindings for Python to send messages back to the message broker.

Django will run in FastCGI mode, and [Nginx][14] will sit in front and proxy traffic to Node or Django based on URL.

I&#8217;ll write more about Rasputin when it&#8217;s more mature, but it&#8217;s designed to build an extremely loosely coupled system. It essentially makes Django an [observer][15], and observable, in a multi-process, multi-language system. When one actor publishes a message, there&#8217;s no guarantee anyone is listening, and there may be multiple people listening.

This architecture will give me a ton of flexibility in the future. I can add or remove services from the message broker without affecting the others, so if I wanted to, for example, replace MySQL with [MongoDB][16] or [CouchDB][17], I could easily build a daemon to start dumping data into the data store without so much as changing a configuration in Django. If I want to replace a component, for example replacing Node with [Twisted][18] for Twitter integration, it&#8217;s as easy as starting one service and stopping another. I can load test new components with production data my subscribing to the right channels.

It&#8217;s taken a long time, but I think this is finally a way of building TodaysMeet that does it right and keeps me interested—attention is what&#8217;s best for users in the end.

 [1]: http://coffeeonthekeyboard.com/the-problem-with-todaysmeet-550/
 [2]: http://todaysmeet.com/
 [3]: http://twitter.com/
 [4]: http://www.djangoproject.com/
 [5]: https://github.com/mozilla/playdoh
 [6]: http://socket.io/
 [7]: http://nodejs.org/
 [8]: http://dev.twitter.com/pages/streaming_api
 [9]: http://sequelizejs.com/
 [10]: http://en.wikipedia.org/wiki/Towelie
 [11]: http://redis.io/
 [12]: http://coffeeonthekeyboard.com/wp-content/uploads/2011/01/TodaysMeetInfra.png
 [13]: https://github.com/jsocol/rasputin
 [14]: http://wiki.nginx.org/Main
 [15]: http://en.wikipedia.org/wiki/Observer_pattern
 [16]: http://www.mongodb.org/
 [17]: http://couchdb.apache.org/
 [18]: http://twistedmatrix.com/trac/