---
title: How TodaysMeet Works
author: James
layout: post
permalink: /how-todaysmeet-works-1237/
pw_single_layout:
  - 1
dsq_thread_id:
  - 3485722579
categories:
  - Articles
tags:
  - django
  - dq
  - ekg
  - javascript
  - node.js
  - python
  - reflektor
  - todaysmeet
---
*I want to write about TodaysMeet&#8217;s 2015 [State of the Union][1] site, but I realized I spent half the time on the existing architecture. So, this is part 1, and [here is part 2][2]!*

A little over two years ago, I set out to [completely replace TodaysMeet&#8217;s platform][3]. Then, [over the past year][4], I&#8217;ve taken the new platform and built it into a distributed, service-oriented—buzzword-compliant—application.

So here it is: how TodaysMeet actually works today. In [part 2][2], I&#8217;ll give a very concrete example of how great this can be.

<img src="http://coffeeonthekeyboard.com/wp-content/uploads/2015/02/How-TodaysMeet-Works.png" alt="a diagram with most of the moving parts" width="486" height="397" class="aligncenter size-full wp-image-1240" />

The two big components are the TodaysMeet Django app, let&#8217;s call it `tm` for now, and the websocket connection service, called `ekg`.

`tm` is written in Python, using Django. It currently serves as both the web app and API, including authenticating for both, though I&#8217;ve been trying to separate those as much as possible. It is solely responsible for talking to the database (and uses memcached where appropriate). Almost all non-static traffic goes through `tm`.

`ekg` is written in JavaScript, using NodeJS ([for now][5]) and [Primus][6] for websocket+fallback connections. The only bi-directional communication is connecting and joining a room, otherwise, everything comes in via the API and goes out via streams. It talks to `tm` via internal API endpoints when it needs information from the database and relies on in-process caching where appropriate.

Let&#8217;s look at the most common case, posting a new comment to a room. Lots of actions (closing or pausing a room, deleting a comment) follow a similar flow.

The `POST` request goes to `tm`. It starts a transaction, writes the comment to the database, then posts the comment to `ekg` before finally committing the transaction. `ekg` pushes the message to all the clients in that room.

Except, it doesn’t post *directly* to `ekg`. During deploys, `ekg` gets restarted, which can take over a second as it stops accepting new connections, tells the current clients to reconnect in a few seconds, shuts down and gets running again. Some posts would fail, causing users to see an error. It&#8217;s also important to scale to multiple `ekg` hosts without making the API slower, meaning it shouldn&#8217;t post to each `ekg` host.

There is an intermediate service called [`reflektor`][7] to solve these problems. It runs on the same boxes as `tm`. `reflektor` accepts any HTTP request and responds immediately, then replays it against a list of downstream servers.

Because it responds immediately, the median time for `tm` to post the comment is just over 3ms. The 90%ile time is 4ms. It doesn&#8217;t matter how many downstream systems `reflektor` will eventually talk to, the `tm` &#8220;new message&#8221; API endpoint is extremely fast.

`reflektor` queues these requests in-process using a library I call `dq` for “dumb queue”—that I swear I will rename and open source or replace at some point. `dq`:

  * stores objects in memory with no persistence options, if it crashes, or is hard-stopped, they are lost;
  * is a FIFO queue, objects get processed in order;
  * has a configurable task to run on each object that can be synchronous or asynchronous and decides what counts as success or failure;
  * uses `setImmediate` or `setTimeout` to avoid blocking the event loop;
  * automatically retries with back off on errors;
  * can be paused or put in “drain” mode;
  * goes to “sleep” when empty to limit CPU time and automatically wakes up on push;
  * and emits events like `'drained'` so I can do graceful restarts.

(There are better, more robust, COTS and FLOSS tools. I built `dq` because I didn’t want the operational overhead of running something like [nsq][8] on small VPSes—I really wanted the queue to run on `localhost` for each `tm` server to limit network time—I wanted to stick to HTTP when possible, and I did not really need its guarantees. But the idea is similar.)

`tm` knows about its local `reflektor` and about those on its neighbors, so if the local `reflektor` is restarting—which happens in serial—`tm` will try a neighbor. Since adding `reflektor` and neighbor awareness, deploys are error-free and unnoticeable.

The biggest requirement for me has been speed—TodaysMeet is a real-time communication tool. So is it fast?

**Yes.** Because `tm` posts the message to `reflektor` *before* it commits the transaction and responds to the request, I actually had to work around a problem when the new message arrives in the browser before the `POST` completes!

I said I&#8217;d talk about how great this is. [Sean][9] has talked about [streams at Bitly][10] a bunch, and nearly everything he said applies here. I&#8217;ll get into it more in [part two][2], but this architecture makes it incredibly easy to build up new systems or features without interfering with what&#8217;s already running.

 [1]: http://blog.todaysmeet.com/sotu2015-on-todaysmeet-426/
 [2]: http://coffeeonthekeyboard.com/visualizing-the-2015-sotu-on-todaysmeet-1246/
 [3]: http://blog.todaysmeet.com/welcome-to-todaysmeet-2-0-140/
 [4]: http://coffeeonthekeyboard.com/irregular-update-04oct2014-1148/
 [5]: https://iojs.org/
 [6]: http://primus.io/
 [7]: https://www.youtube.com/watch?v=7E0fVfectDo
 [8]: http://bitly.github.io/nsq/
 [9]: https://twitter.com/theseanoc
 [10]: https://www.hakkalabs.co/articles/bitlys-practical-strategies-building-distributed-systems