---
title: 'Server Configuration &#8211; Basic Security Part 7'
author: James
layout: post
permalink: /server-configuration-basic-security-part-7-816/
bitly_url:
  - http://j0.is/14dZNYD
bitly_hash:
  - 14dZNYD
bitly_long_url:
  - http://coffeeonthekeyboard.com/server-configuration-basic-security-part-7-816/
dsq_thread_id:
  - 1746519761
categories:
  - Security
tags:
  - django
  - django-nyc-security
  - mozilla
  - security
  - servers
---
*NB: This is the seventh post in a [series][1] of posts on web application security.*

Configuring a server correctly is both 1) hard and 2) critical.

You&#8217;ve probably spent a bunch of time configuring [Apache][2] or [nginx][3], or whatever your server of choice is, for performance. But have you configured it for security?

I can&#8217;t tell you exactly what to do without knowing your set up, but some basics:

  1. Are directories only writeable by the web <del>server</del><ins>application</ins> user<sup>1</sup>?
  2. Do all of them even need to be? Are you sure?
  3. Can the web process write to its own source files?
  4. Are any `Alias` or `ScriptAlias` directives set you don&#8217;t know about?
  5. Are your firewall rules restrictive enough?
  6. There is literally so much more. Contract a good sysadmin.
  7. Is PHP installed on your Python server?

<sup>1</sup>: See Valentin&#8217;s comment about running server and application(s) as separate users. He&#8217;s right.

Let me [elaborate on the last one][4], because our security team will let me. We had left PHP installed (part of our puppet configs) on app servers that were only going to run Python. Someone discovered a small hole&mdash;we weren&#8217;t checking the extensions of uploaded images&mdash;and realized they could upload PHP scripts, and the Apache server, happily serving &#8220;static&#8221; files, interpreted and ran them.

**Shit.**

Don&#8217;t do that. Learn from us on that one. Double check. Then check again.

## What else?

Those of you who&#8217;ve configured servers, webdevs, devops, sysadmins, what are other key things to check to make sure you&#8217;ve hardened your server configuration?

  * Go to the [series index][5].
  * Go to the previous post in the series, [Session Fixation and Hijacking][6].
  * Go to the next post in the series, [Click-Jacking and a little Phishing][7].

 [1]: http://coffeeonthekeyboard.com/best-basic-security-practices-especially-with-django-697/
 [2]: http://httpd.apache.org/
 [3]: http://nginx.org/
 [4]: https://bugzilla.mozilla.org/show_bug.cgi?id=619467
 [5]: http://coffeeonthekeyboard.com/best-basic-security-practices-especially-with-django-697/ "Best Basic Security Practices (Especially with Django)"
 [6]: http://coffeeonthekeyboard.com/session-fixation-and-hijacking-basic-security-part-6-801/ "Session Fixation and Hijacking – Basic Security Part 6"
 [7]: http://coffeeonthekeyboard.com/click-jacking-and-a-little-phishing-basic-security-part-8-824/ "Click-Jacking and a little Phishing – Basic Security Part 8"