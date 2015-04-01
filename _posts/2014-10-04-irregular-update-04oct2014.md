---
title: Irregular Update 04/Oct/2014
author: James
layout: post
permalink: /irregular-update-04oct2014-1148/
pw_single_layout:
  - 1
dsq_thread_id:
  - 3082588446
categories:
  - Articles
tags:
  - weekly update
---
I&#8217;ve been thinking about restarting the [weekly updates][1] I used to do, mostly just to get myself in the habit of writing more, but also to make myself take some note of what I&#8217;ve gotten done.

It has been a [very busy summer][2], since I started [working full-time on TodaysMeet][3]. But as long as that list is, there are a bunch of under-the-hood changes I haven&#8217;t been able to talk about much yet. And I like being as open as I can with technical things.

The biggest change is the switch from periodic short polling (which produced too many requests per second for Linode&#8217;s NodeBalancers) to **streaming connections**.

The addition of streaming connections also marked the official switch of TodaysMeet from a [monolithic Django app][4] to a multi-service architecture&#x2020;. Having spent time with Django and gevent working on chat prototypes for Mozilla Help [way back in the day][5], I went with Node.js and [Primus][6], which has been great&#x2a;.

To keep the API responses fast (90%ile is under 50ms, plus network time) and to facilitate horizontal scaling for the streaming processes, I built a new intermediary process, called &#8220;reflektor&#8221; (because [I&#8217;m a dork][7]). Reflektor accepts HTTP requests and returns instantly, while queuing them for replay against one or more hosts. The API servers post to reflektor (90%ile, 3ms) which posts to the streaming service (&#8220;ekg&#8221;—see? dork).

I am planning, once I get to a breather moment, to open-source a lot of the internal node packages I&#8217;ve written. The in-process task queue from reflektor is very close to ready to go, and I&#8217;ve done some work with logging, porting much of Python&#8217;s logging module (which is itself a port of log4j, and which I&#8217;m sure dozens of others have done, but&#8230;) and on HTTP host pools. All of which I&#8217;ve written to be stand-alone and open-able.

Streaming connections have allowed me to do all sorts of other things. The most user-visible of which is [deleting comments][8] from rooms in a meaningful way.

To keep the UI responsive and get the streams started faster, I&#8217;ve started loading non-streaming messages lazily, with infinite scroll. This made some of the logic (&#8220;which order do messages come in? do I want messages *since* or messages *before*?&#8221;) fairly complex. Let&#8217;s all be thankful for tests! Limiting the number of messages per response took nearly all the spikiness out of the API response times and made them much faster. Here&#8217;s the 90%ile for two weeks either site of the switch.

<img src="http://coffeeonthekeyboard.com/wp-content/uploads/2014/10/room-messages-90pctile.png" alt="" width="479" height="370" class="aligncenter size-full wp-image-1152" />

Because I&#8217;ve started handling [webhook requests][9] from Mandrill, MailChimp, and now Stripe(!), I&#8217;ve been building a generic recording mechanism to prevent event replay, and that&#8217;s something that may make sense to open up, if it turns out to be generically useful. But given the lack of standardization of webhook payloads, it&#8217;s tough to do anything that&#8217;s both generic and interesting.

I&#8217;ve taken to building my own RPMs of more and more essential libraries, like [nginx and OpenSSL][10] and node, and even py2cairo, which is a pain to install normally.

All of which has meant I&#8217;ve done a bunch of work on deployments, specifically zero-downtime deployments. Almost all of that has been in Fabric. No one on the site should notice, or see an error, during a deployment, no matter what they try to do.

And the deployments work well! Having spent so much time working on [continuous deployment][11] at Mozilla and using the fantastic deployment systems at Bitly, I&#8217;m pretty proud of the hundreds of non-event deployments that I&#8217;ve done over the summer. Most of them during peak traffic, because it&#8217;s when I&#8217;m most awake if anything goes wrong.

One of the biggest challenges of running a one-person show is doing *everything*. Along with my lovely eng/devops/releng hat, I&#8217;ve spent an uncomfortable amount of time playing designer, [social media intern][12], product designer, and, as I get ready to roll out the [first paid TodaysMeet product][13], business person. [Josh][14] has been an invaluable friend and resource throughout. If you need design or product help, or development, you should [hire him][15].

It&#8217;s been a busy summer. Even busier than the [TodaysMeet blog][16] lets on. [Now it&#8217;s autumn][17], and with any luck it will be just as busy.

I want to write more, about some of this stuff in detail, and hopefully give some shorter talks about it in the area. And open source some software! So I&#8217;m going to do my best to make some time to do that. I can&#8217;t promise that I&#8217;ll totally restart weekly updates but at the very least I can do semi-regular little brain dumps like this.

&#x2a; Since I chose Primus, [Socket.io][18] has released a new major version.

&#x2020; Not including things like StatsD, Graphite, and email.

 [1]: http://coffeeonthekeyboard.com/tag/weekly-update/
 [2]: http://blog.todaysmeet.com/a-busy-july-at-todaysmeet-304/
 [3]: http://coffeeonthekeyboard.com/whats-next-1113/
 [4]: http://coffeeonthekeyboard.com/why-django-sucks-except-when-it-doesnt-664/
 [5]: https://wiki.mozilla.org/Support/SumoDevRoadmap2009
 [6]: https://github.com/primus/primus
 [7]: https://www.youtube.com/watch?v=7E0fVfectDo
 [8]: http://blog.todaysmeet.com/moderate-your-rooms-273/
 [9]: http://coffeeonthekeyboard.com/best-practices-for-happy-webhooks-1138/
 [10]: http://coffeeonthekeyboard.com/patching-heartbleed-1106/
 [11]: http://coffeeonthekeyboard.com/tag/continuous-deployment/
 [12]: https://twitter.com/TodaysMeet
 [13]: https://todaysmeet.com/about/teachertools
 [14]: http://gross.is/
 [15]: http://planetary.io/
 [16]: http://blog.todaysmeet.com
 [17]: http://coffeeonthekeyboard.com/things-i-love-about-fall-474/
 [18]: http://socket.io/