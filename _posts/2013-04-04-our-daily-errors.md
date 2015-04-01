---
title: Our Daily Errors
author: James
layout: post
permalink: /our-daily-errors-941/
pw_single_layout:
  - 1
bitly_url:
  - http://j0.is/16FvqiP
bitly_hash:
  - 16FvqiP
bitly_long_url:
  - http://coffeeonthekeyboard.com/our-daily-errors-941/
dsq_thread_id:
  - 1746545528
categories:
  - Articles
---
Over the past 24 hours, [support.mozilla.org][1] has recorded a few dozen errors. That&#8217;s pretty good.

In the past three days, it&#8217;s more like a few hundred.

These are sporadic, little, intermittent errors that, for most practical purposes, don&#8217;t happen. The odds are so low that you only see them when you look at all the traffic to a reasonably busy website.

Almost every user registration form has a race condition in it. If two people try to register the same username at the same time, there are a few milliseconds between checking that the name is available and writing a new row to the database. The odds of that are terrible, though. Right? One out of every few thousand requests gets set twice. The web app picks up both and two threads, maybe even two processes on different web servers, happily start processing the registration. One of them gets to the INSERT first, and the other one fails.

This is preventable, but, well, meh. The user is still registered. The &#8220;welcome&#8221; email is still sent.

Some are the sort of weird behavior you only get on a big, complicated network, like the internet. The server tried to finish reading the request packet and&mdash;oh, well, it isn&#8217;t there. A connection was reset by a peer. Or an internal connection dropped and reconnected, and one request was in the middle of that.

Some are the things you get when you run a busy server. It&#8217;s rare, but when you do the same read operation 10 or 100 thousand times, sometimes it just doesn&#8217;t work. Nothing wrong with the file. The next time works fine. It happens.

And some are legitimate bugs. But they&#8217;re esoteric and hard to trigger and we know they&#8217;re there but they just aren&#8217;t that big a deal. There are other, more important things.

There are always errors. You get used to it.

 [1]: https://support.mozilla.org