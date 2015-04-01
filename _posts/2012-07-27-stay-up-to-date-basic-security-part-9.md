---
title: 'Stay Up to Date &#8211; Basic Security Part 9'
author: James
layout: post
permalink: /stay-up-to-date-basic-security-part-9-834/
bitly_url:
  - http://j0.is/14dZLjq
bitly_hash:
  - 14dZLjq
bitly_long_url:
  - http://coffeeonthekeyboard.com/stay-up-to-date-basic-security-part-9-834/
categories:
  - Security
tags:
  - django
  - django-nyc-security
  - mozilla
  - security
---
*NB: This is the ninth post in a [series][1] of posts on web application security.*

Rounding out this week is the last, but perhaps most important part of the **basic** security series: staying up to date.

Keeping everything up-to-date is a pain. You have to follow the latest versions of everything you use. And when things *just work*, it&#8217;s hard to justify taking the time to upgrade part of it and make them *just work* again.

Too bad: **it&#8217;s critical**. A lot of the Django features I mentioned are only in Django 1.4. Though for the big things, like `X-Frame-Options`, writing or [finding][2] code for older versions isn&#8217;t too hard.

But the small version increases, the 0.0.1 bumps, those contain [important][3] [security fixes][4], and rarely break backwards compatibility.

  1. Stay as up-to-date as you can. Don&#8217;t end up on a deprecated major revision or version.
  2. Absolutely keep the patch-level up-to-date. That&#8217;s where security fixes land. Not just in Django or Rails, but in lots of little libraries you&#8217;ve probably forgotten about.

Next week, we&#8217;ll dive into some more advanced gotchas and security issues, mostly from my own experience over the past couple of years. Until then, have a great weekend, and don&#8217;t forget to lock your doors.

  * Go to the [series index][5].
  * Go to the previous post in the series, [Click-jacking and a little Phishing][6].
  * Go to the next post in the series, [Mass Assignment][7].

 [1]: http://coffeeonthekeyboard.com/best-basic-security-practices-especially-with-django-697/
 [2]: https://github.com/jsocol/commonware/blob/master/commonware/response/middleware.py#L21
 [3]: https://www.djangoproject.com/weblog/2011/sep/09/security-releases-issued/
 [4]: http://weblog.rubyonrails.org/2012/3/30/ann-rails-3-2-3-has-been-released/
 [5]: http://coffeeonthekeyboard.com/best-basic-security-practices-especially-with-django-697/ "Best Basic Security Practices (Especially with Django)"
 [6]: http://coffeeonthekeyboard.com/click-jacking-and-a-little-phishing-basic-security-part-8-824/
 [7]: http://coffeeonthekeyboard.com/mass-assignment-security-part-10-855/ "Mass Assignment – Security Part 10"