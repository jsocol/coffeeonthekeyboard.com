---
title: 'Mozilla&#8217;s Security Best Practices'
author: James
layout: post
permalink: /mozillas-security-best-practices-873/
bitly_url:
  - http://j0.is/QjdNwX
bitly_hash:
  - QjdNwX
bitly_long_url:
  - http://coffeeonthekeyboard.com/mozillas-security-best-practices-873/
categories:
  - Articles
tags:
  - django
  - djangocon
  - mozilla
  - security
---
This list of resources is meant as a companion to the talk I gave at DjangoCon 2012, but it should stand on its own as a useful list for Django developers.

## Best Practices?

What are &#8220;best practices,&#8221; anyway? The internet loves to debate these things. For us, think of it as the collective team knowledge, condensed into things like [docs][1]; shared, [reviewed libraries][2]; [application templates][3]; code review standards; and user experience guidelines.

## Docs

  * [WebDev Bootcamp][1]
  * [Localization][4]
  * [Playdoh Docs][5]

## Libraries

  * [Playdoh][3] &#8211; Our application template.
  * [Funfactory][2] &#8211; Core of our shared stuff, base settings.
  * [django-sha2][6] &#8211; HMAC+Bcrypt password storage.
  * [Bleach][7] &#8211; HTML sanitizer and link-finder.
  * [django-session-csrf][8] &#8211; Better CSRF for subdomains.
  * [django-ratelimit][9] &#8211; Ratelimiting decorator.
  * [commonware][10] &#8211; Some other useful middleware and monkeypatches.
  * [tower][11] &#8211; Our wrapper around [Babel][12].
  * [jingo][13] &#8211; Our adapter to use [Jinja2][14] templates, with some built-in filters.
  * [cef][15] &#8211; Common Event Format logger for security events.
  * [django-csp][16] &#8211; [CSP][17] header tool.

## Django Features

  * [CSRF][18] &#8211; Django&#8217;s CSRF protection.
  * [X-Frame-Options][19] &#8211; X-Frame-Options tools.
  * [Secure Sessions][20] &#8211; Set session cookies for HTTPS only.

 [1]: http://mozweb.readthedocs.org/en/latest/index.html
 [2]: https://github.com/mozilla/funfactory
 [3]: https://github.com/mozilla/playdoh
 [4]: http://kitsune.readthedocs.org/en/latest/localization.html
 [5]: http://playdoh.readthedocs.org/en/latest/
 [6]: https://github.com/fwenzel/django-sha2
 [7]: https://github.com/jsocol/bleach
 [8]: https://github.com/mozilla/django-session-csrf
 [9]: https://github.com/jsocol/django-ratelimit
 [10]: https://github.com/jsocol/commonware
 [11]: https://github.com/clouserw/tower
 [12]: http://pypi.python.org/pypi/Babel
 [13]: https://github.com/jbalogh/jingo
 [14]: http://jinja.pocoo.org/
 [15]: http://pypi.python.org/pypi/cef/0.3
 [16]: https://github.com/mozilla/django-csp
 [17]: http://www.w3.org/TR/CSP/
 [18]: https://docs.djangoproject.com/en/1.4/ref/contrib/csrf/
 [19]: https://docs.djangoproject.com/en/1.4/ref/clickjacking/
 [20]: https://docs.djangoproject.com/en/1.4/topics/http/sessions/#session-cookie-secure