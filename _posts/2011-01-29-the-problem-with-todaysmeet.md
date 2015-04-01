---
title: The Problem with TodaysMeet
author: James
layout: post
permalink: /the-problem-with-todaysmeet-550/
bitly_url:
  - http://j0.is/14dZwVB
bitly_hash:
  - 14dZwVB
bitly_long_url:
  - http://coffeeonthekeyboard.com/the-problem-with-todaysmeet-550/
dsq_thread_id:
  - 1746519682
categories:
  - Articles
tags:
  - architecture
  - Design
  - framework
  - javascript
  - PHP
  - todaysmeet
  - webdev
---
[TodaysMeet][1] is a project I started in 2008 to help [my father][2] solve a problem in one of his classes. The fact that it&#8217;s as popular as it is—mostly in education—never ceases to amaze me.

Unfortunately, I don&#8217;t give TodaysMeet the attention it, and more importantly its users, deserve. This is because TodaysMeet has two fatal flaws that, if they haven&#8217;t crippled it yet, will someday.

  * The UI is based on proof-of-concept JavaScript.
  * The back-end is based on [my own framework][3].

What follows is the sad history of TodaysMeet development.

### Origin Story

TodaysMeet came out of a conversation between my father and I, but it&#8217;s origins are slightly older. In some downtime in late 2007 I was trying to familiarize myself with various JavaScript frameworks by writing a UI for the same back-end in each of them. It was a pretty basic Ajax comment system. I believe it polled the server every minute. If I remember correctly, I got busy and abandoned it after creating [Prototype][4] and [jQuery][5] versions.

Around the same time I was enamored of [Rails][6], and trying to round out [Maveric][3] into a decent Rails-inspired PHP framework.

So when my father said he wanted something like Twitter for a single classroom, that he could project on a wall, and wouldn&#8217;t require signing up, I put these things together in my head. TodaysMeet is basically the proof-of-concept Prototype JS running on top of an old version of Maveric.

### The Situation Now

[Every developer should write a framework][7], I think it&#8217;s a fantastic learning experience. But they should never build a production website out of it.

Even though Maveric got a little better after I created TodaysMeet, it&#8217;s still based on an untested, unsupported framework with no support for basic things like storage back-ends or caching.

The UI is still based on Prototype, which I haven&#8217;t used in years, and the fundamental client-server interactions are still that original &#8220;learning the library&#8221; code.

Essentially, TodaysMeet is a prototype masquerading as a production-ready product.

The result is that working on it is slow, difficult, and frankly unpleasant. Adding features—like the long-promised password protected rooms—is painful and, with no test suite, dangerous. The one real feature I added, Twitter integration, barely works when it works at all.

But users don&#8217;t care about any of that. They see that it works, mostly. They might see that it doesn&#8217;t get much attention and the UI feels three years old (because it is, of course).

### Where Do We Go From Here?

TodaysMeet *could* be awesome, but it needs to go all the way down to the basic stack and get rebuilt. TodaysMeet is an absolutely perfect candidate for all sorts of new, exciting tools and techniques. To use any of them means starting over.

This is the first of a two-part post. In the next part, I&#8217;m going to outline the architecture I *want*, instead of the architecture I *have*.

Hopefully, some social aspect of talking about this will lead to me actually doing something about it.

 [1]: http://todaysmeet.com/
 [2]: http://speedchange.blogspot.com
 [3]: http://svn.jamessocol.com/maveric
 [4]: http://www.prototypejs.org/
 [5]: http://jquery.com/
 [6]: http://rubyonrails.org/
 [7]: http://www.brandonsavage.net/why-every-developer-should-write-their-own-framework/