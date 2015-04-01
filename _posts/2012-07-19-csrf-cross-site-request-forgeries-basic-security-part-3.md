---
title: 'CSRF: Cross-Site Request Forgeries &#8211; Basic Security Part 3'
author: James
layout: post
permalink: /csrf-cross-site-request-forgeries-basic-security-part-3-747/
bitly_url:
  - http://j0.is/14e0mBR
bitly_hash:
  - 14e0mBR
bitly_long_url:
  - http://coffeeonthekeyboard.com/csrf-cross-site-request-forgeries-basic-security-part-3-747/
dsq_thread_id:
  - 1746519714
categories:
  - Security
tags:
  - csrf
  - django
  - django-nyc-security
  - mozilla
  - security
---
*NB: This is the third post in a [series][1] of posts on web application security.*

The quintessential example of a CSRF (sometimes pronounced &#8220;sea-surf&#8221;) is a bank that naively does transfers over a `GET` request without any other security:

    http://badbank.com/transfer?from=act1&to=act2&amt=100000.00
    

Ignoring how many other bad things are going on here, there&#8217;s no validation that the request is coming from a site. I could include an image tag somewhere with that URL and every time someone visited the site, I&#8217;d get another hundred grand.

Django, and most frameworks, have [built-in CSRF protection][2] that uses the concept of a *nonce*, or one-time-use number. These are submitted with a form (over `POST`, hopefully) and compared to a number on the server-side. If the number doesn&#8217;t match, the request is prohibited.

**Update:** Because of [an insightful comment][3] on HN, I want to make it clear that using `POST` does not guarantee safety from CSRF. Someone can use an `<iframe>` and execute a `POST` request. But it does two things:

  * It reduces the surface area of attack. There are a lot more places I can stick an `<img>` tag that can trigger a `GET` request on high-traffic parts of the web than there are places I can stick some code to create an `iframe` and do a `POST`.
  * It&#8217;s a better practice to do make any *change* a `POST` because all the actors, from the browser/user-agent, to proxies, to load-balancers, to web servers, understand that the `POST` verb means something is changing and treat it differently than most `GET` requests.

Whatever your framework provides for this, **use it**.

If you&#8217;re using Django, you may also want to look at [django-session-csrf][4], which provides a bit more security than the default implementation, which uses cookies. It stores the actual CSRF token (nonce) in the session, instead of in its own cookie. This provides better protection against compromised apps on different subdomains and MITM attacks that can alter cookie values.

## Testing CSRF Protection

Testing CSRF protection is tricky, especially in Django. The built-in test client ignores CSRF protections, so you may break parts of your site without knowing.

Using something like [Selenium][5] to drive tests through form submissions in actual browsers can help guarantee that you both have CSRF protection in place and that you&#8217;re providing all the credentials and nonces correctly to the browser.

(And yes, we&#8217;ve stumbled over and broken that before. It took us a few days to track down all the forms we broke.)

  * Go to the [series index][6].
  * Go to the previous post in the series, [XSS: Cross-Site Scripting][7].
  * Go to the next post in the series, [Injections, SQL and otherwise][8].

 [1]: http://coffeeonthekeyboard.com/best-basic-security-practices-especially-with-django-697/
 [2]: https://docs.djangoproject.com/en/dev/ref/contrib/csrf/
 [3]: http://news.ycombinator.com/item?id=4266244
 [4]: https://github.com/mozilla/django-session-csrf
 [5]: http://seleniumhq.org/
 [6]: http://coffeeonthekeyboard.com/best-basic-security-practices-especially-with-django-697/ "Best Basic Security Practices (Especially with Django)"
 [7]: http://coffeeonthekeyboard.com/xss-cross-site-scripting-basic-security-part-2-711/ "XSS: Cross-Site Scripting – Basic Security Part 2"
 [8]: http://coffeeonthekeyboard.com/injections-sql-and-otherwise-basic-security-part-4-755/ "Injections, SQL and otherwise – Basic Security Part 4"