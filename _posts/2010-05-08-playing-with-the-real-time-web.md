---
title: Playing with the Real-Time Web
author: James
layout: post
permalink: /playing-with-the-real-time-web-412/
bitly_url:
  - http://j0.is/d2FgUb
bitly_hash:
  - d2FgUb
bitly_long_url:
  - http://coffeeonthekeyboard.com/playing-with-the-real-time-web-412/
categories:
  - Articles
tags:
  - django
  - node.js
  - realtime
  - redis
  - Social Networking
  - webdev
---
Since [I left Facebook][1], I&#8217;ve been working on a little project to take its place as an aggregator, or a central hub of all me-related activity on the internet: [Planetoid][2].

Right now, if you&#8217;ve got the patience to get it up and running without documentation, Planetoid is just a run of the mill, particularly ugly feed aggregator. It&#8217;s built on Django, and it has a cron job that well pull in updates from all your feeds. (You edit the list of feeds in the Django admin.)

But that&#8217;s boring.

This is just the platform I needed. Now, I want to make it real-time.

The first step is implementing [Pubsubhubbub][3] subscriber support. Then the cron will only be necessary for feeds that don&#8217;t push update notifications.

The next step is where things get interesting: both the cron and pubsubhubbub subscriber will push notifications using [Redis][4]&#8216; [pubsub][5] feature. [Node.js][6], running in parallel, will subscribe to the channel in Redis and will provide the server half of a [Comet/long-polling][7] setup, the rest of which will be implemented on the client side.

Then I just need to [enable pubsubhubbub in a few places][8], and anyone sitting on [jamessocol.com][9] should see things like blog posts in real-time, and everything else automatically with a small lag.

Real-time and the tools to do it are very, very fun.

 [1]: http://coffeeonthekeyboard.com/farewell-facebook-410/
 [2]: http://github.com/jsocol/planetoid
 [3]: http://code.google.com/p/pubsubhubbub/
 [4]: http://code.google.com/p/redis/
 [5]: http://code.google.com/p/redis/wiki/PublishSubscribe
 [6]: http://nodejs.org/
 [7]: http://en.wikipedia.org/wiki/Comet_(programming)#XMLHttpRequest_long_polling
 [8]: http://wordpress.org/extend/plugins/pubsubhubbub/
 [9]: http://jamessocol.com/