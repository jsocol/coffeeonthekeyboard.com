---
title: 'Why Django Sucks, Except When It Doesn&#8217;t'
author: James
layout: post
permalink: /why-django-sucks-except-when-it-doesnt-664/
bitly_url:
  - http://j0.is/14e0jpA
bitly_hash:
  - 14e0jpA
bitly_long_url:
  - http://coffeeonthekeyboard.com/why-django-sucks-except-when-it-doesnt-664/
dsq_thread_id:
  - 1746519491
categories:
  - Articles
tags:
  - Code
  - django
  - mozilla
  - scaling
---
[Ken Reitz][1] is a smart man. Very smart. Smarter than me. He&#8217;s responsible for some of the [best][2], [most widely-used][3] Python libraries out there. So when he talks, I listen.

And recently, he talked about [why Django sucks][4].

Ken made some really excellent points. Among them:

  * Django is too monolithic, and so&#8230;
  * it encourages big, tightly-coupled apps.
  * Everything depends on the Django ORM, whether it&#8217;s front-end stuff like sessions or back-end stuff like data persistence.
  * Broad knowledge of the system is required.
  * All components get deployed together.

He also had plenty of nice things to say about Django, but (and Ken, correct me if I&#8217;m wrong) the point seemed to be that we should take a page from [Steve Yegge&#8217;s infamous rant][5] and try to separate concerns (e.g. why do user sessions have anything to do with data persistence?) which means building smaller silos that talk over APIs (maybe HTTP, because it&#8217;s easy).

These silos are easier to understand, can be deployed, scaled, and updated independently, provide more opportunities for fault-tolerance, and have a number of other advantages.

So why on earth would you just use Django as-is?

Because for a lot of people, **none of that matters**.

If you&#8217;re prototyping a new project, or if you&#8217;re a start-up and need to ship *something*, or if you&#8217;re just one person working on a little app with a few (even a few tens of thousands) of users, odds are that none of those benefits are worth losing the nice stuff Django gives you, or the time it takes to build APIs (even using existing apps to do it) or abstraction layers between the silos.

A while ago, someone tweeted something to the effect of &#8220;while you were arguing about Rails vs Django vs Flask vs Erlang, a bunch of people shipped with PHP.&#8221; That&#8217;s my sentiment here. 99% of the time, for 99% of web apps, the benefits of doing things the &#8220;right&#8221; way are just never going to show up. Start with the easy, fast way.

Twitter is the perfect example. Today, the website is a web client that talks over APIs to the real messaging service and yadda yadda yadda, but on launch day, it was a Rails app, and, from the outside, it didn&#8217;t even seem like a particularly clever Rails app.

Django (or Rails, or even Flask with some packages from its ecosystem) gives you a lot in the box. Unless you know that you&#8217;re launching to millions of users, it&#8217;s OK to take that box and start building.

If you end up in a place and at a scale where that&#8217;s technical debt you *really need* to pay off: congratulations!

 [1]: https://twitter.com/kennethreitz
 [2]: https://github.com/kennethreitz/flask-sslify
 [3]: https://crate.io/packages/requests/
 [4]: https://speakerdeck.com/u/kennethreitz/p/flasky-goodness
 [5]: https://plus.google.com/112678702228711889851/posts/eVeouesvaVX