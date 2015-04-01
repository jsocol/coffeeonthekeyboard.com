---
title: 'Developing at Scale: Database Replication'
author: James
layout: post
permalink: /developing-at-scale-database-replication-444/
bitly_url:
  - http://j0.is/14dZwVE
bitly_hash:
  - 14dZwVE
bitly_long_url:
  - http://coffeeonthekeyboard.com/developing-at-scale-database-replication-444/
dsq_thread_id:
  - 1746519693
categories:
  - Articles
tags:
  - Back-end
  - Code
  - Database
  - mozilla
  - programming
  - sumo
  - webdev
---
When a website is small—like this one, for example—usually the entire thing, from the web server to the database, can live on a single server. Even a single virtual server. One of the first things that happens when a web site gets bigger is this is no longer true.

One reason is load. A popular website will simply require more than a single server, virtual or otherwise, can give, and the only way to keep scaling is to add more servers. For example, if the server runs out of available Apache connections and the number cannot be raised without negatively impacting performance.

Another reason is downtime. If a website is served from a single server, and that server goes down for any reason, planned or otherwise, then the website is down. At some point, downtime is essentially unacceptable—just ask Twitter—and redundancy is required.

### Enter Replication

A common response is to set up database replication, where one database server operates as a &#8220;master,&#8221; and one or more other servers operate as &#8220;slaves.&#8221; In this setup, all of your *writes* to the database will go to the master, then &#8220;replicate&#8221; to the slaves, and all or most of the *reads* will come from the slaves. (Note that the slaves are doing both all the writes as well as all the reads: slaves are not a good place to recycle sub-par hardware.)

Replication introduces a new type of problem: if you naively send *all* reads to the slaves then data you just wrote *will not be there*.

### La&#8230;wait for it&#8230;g

Even if the master and slave are sitting next to each other with a cable connecting them, replication will probably take more time than your code does to reach the next step. At a minimum, you need to assume that replication lag will be hundreds of milliseconds—an eternity when the time from one line in your web app to the next is measured in micro- or nanoseconds. In reality, replication in the real world may well take seconds, especially if your master and slaves are not physically next to each other.

The result is that [ACIDity][1] is essentially broken, specifically the **D**urability part. You cannot simply write data and immediately rely on its existence.

For example, say you have a large discussion forum. If you naively send all reads to the slaves, then someone&#8217;s post may take seconds to appear on the site. This is a problem if you&#8217;re trying to show a user their post immediately after posting it.

### Smarter Reading

The solution is to occasionally read from the master. When you need to access data that was just written, it is *probably* only available on the master, so that&#8217;s where you&#8217;ll read it. Within a single HTTP request, this is fairly simple: just force any queries that rely on recently-written data to the master.

Outside of a single HTTP request, this is slightly more complex. If you&#8217;re following the practice of redirecting after a POST request to a GET request (which you should) then creating a new forum post and viewing it will be on two different HTTP requests.

One way around this is to set a very short-lived cookie that tells your web app to continue reading from the master. If any write occurs in a request, the response should include this cookie. The exact time-to-live will depend on how long your replication lag usually is—cover at least 4 or 5 standard deviations. Any request that has this cookie should honor it by reading only from the master.

### A Pitch

One of the hardest things for new web developers is developing large-scale applications: first, you need a large-scale application! Setting up database replication is a huge pain, and if your site isn&#8217;t getting enough traffic, it&#8217;s not worth it.

Mozilla is one way aspiring web developers can get some experience working with large-scale web apps. All of our web apps are open source and open to contributions from community members. To get involved, stop by [#webdev][2] in IRC!

 [1]: http://en.wikipedia.org/wiki/ACID
 [2]: irc://irc.mozilla.org/webdev