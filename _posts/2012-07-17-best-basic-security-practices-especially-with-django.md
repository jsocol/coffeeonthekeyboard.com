---
title: Best Basic Security Practices (Especially with Django)
author: James
layout: post
permalink: /best-basic-security-practices-especially-with-django-697/
bitly_url:
  - http://j0.is/S7bPg5
bitly_hash:
  - S7bPg5
bitly_long_url:
  - http://coffeeonthekeyboard.com/best-basic-security-practices-especially-with-django-697/
dsq_thread_id:
  - 1746519622
categories:
  - Security
tags:
  - django
  - django-nyc-security
  - mozilla
  - security
---
# Or: Locking Your Doors

*This goes along with a talk I gave at [Django-NYC][1] in July 2012, but is meant to stand on its own. It is the first in a series of posts, because I realized it was too big for one.*

Security is proportional. Most apps don&#8217;t need two-factor auth&mdash;some certainly do&mdash;but there is a set of common attacks, easily mitigated, that basically any transactional web application is vulnerable to.

Covering these basics is like locking your car doors. For most cars, a thief is just going to try the handle and then move on. If you&#8217;re driving a Ferrari or have a bag of cash sitting in the back seat&mdash;if you&#8217;re a bank or a high-profile target&mdash;you&#8217;re going to need to be more proactive.

At Mozilla, we&#8217;ve rolled a lot of this into [Playdoh][2], our Django application template, and [funfactory][3], a Django app that actually holds a lot of the code.

These best-practices are locking your doors. If your site is a high-profile target or handles financial data, you&#8217;ll need to [go beyond this][4].

## OWASP

Before I go on, a *fantastic* resource for web app developers is [OWASP][5]. The group maintains a ton of great info about common and emerging attacks against web apps, how to mitigate attack vectors, and more. They&#8217;re worth bookmarking, following, even joining.

## The Series

This is the first post in a series. The series will cover what I covered in the talk, but it&#8217;s too big for a single blog post, so I&#8217;m breaking it up into a series of posts that will go up this week and next. The basic structure is:

  * Basics: locking your car doors. 
      * [Password Storage][6]
      * [XSS: Cross-Site Scripting][7]
      * [CSRF: Cross-Site Request Forgeries][8]
      * [Injections, SQL and Otherwise][9]
      * [Access Control][10]
      * [Session Fixation and Hijacking][11]
      * [Server Configuration][12]
      * [Click-jacking and a little Phishing][13]
      * [Stay Up to Date][14]
  * Advanced: Some gotchas from my experience and some things you may well see. 
      * [Mass Assignment][15]
      * Cache Poisoning
      * Bots: Spam, Brute-force, and User Experience
      * PCI-DSS
      * CEF Logging
  * What browsers are doing to help. 
      * Content Security Policy
      * X-Frame-Options
      * Do Not Track
      * Sandboxing

Over the next week or so, I&#8217;ll fill in that outline with links to the individual posts, so if you want to bookmark this one, it&#8217;s not a bad place to start. Or look at the [security tag][4] on this blog.

 [1]: http://www.djangonyc.org/events/70626822/
 [2]: http://playdoh.readthedocs.org/en/latest/index.html
 [3]: https://github.com/mozilla/funfactory
 [4]: http://coffeeonthekeyboard.com/tag/security/
 [5]: https://www.owasp.org/
 [6]: http://coffeeonthekeyboard.com/password-storage-basic-security-part-1-706/ "Password Storage – Basic Security Part 1"
 [7]: http://coffeeonthekeyboard.com/xss-cross-site-scripting-basic-security-part-2-711/ "XSS: Cross-Site Scripting – Basic Security Part 2"
 [8]: http://coffeeonthekeyboard.com/csrf-cross-site-request-forgeries-basic-security-part-3-747/ "CSRF: Cross-Site Request Forgeries – Basic Security Part 3"
 [9]: http://coffeeonthekeyboard.com/injections-sql-and-otherwise-basic-security-part-4-755/ "Injections, SQL and otherwise – Basic Security Part 4"
 [10]: http://coffeeonthekeyboard.com/access-control-basic-security-part-5-765/ "Access Control – Basic Security Part 5"
 [11]: http://coffeeonthekeyboard.com/session-fixation-and-hijacking-basic-security-part-6-801/ "Session Fixation and Hijacking – Basic Security Part 6"
 [12]: http://coffeeonthekeyboard.com/server-configuration-basic-security-part-7-816/ "Server Configuration – Basic Security Part 7"
 [13]: http://coffeeonthekeyboard.com/click-jacking-and-a-little-phishing-basic-security-part-8-824/ "Click-Jacking and a little Phishing – Basic Security Part 8"
 [14]: http://coffeeonthekeyboard.com/stay-up-to-date-basic-security-part-9-834/
 [15]: http://coffeeonthekeyboard.com/mass-assignment-security-part-10-855/ "Mass Assignment – Security Part 10"