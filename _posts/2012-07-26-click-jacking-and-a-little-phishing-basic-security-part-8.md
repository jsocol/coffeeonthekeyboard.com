---
title: 'Click-Jacking and a little Phishing &#8211; Basic Security Part 8'
author: James
layout: post
permalink: /click-jacking-and-a-little-phishing-basic-security-part-8-824/
bitly_url:
  - http://j0.is/14dZLA9
bitly_hash:
  - 14dZLA9
bitly_long_url:
  - http://coffeeonthekeyboard.com/click-jacking-and-a-little-phishing-basic-security-part-8-824/
categories:
  - Security
tags:
  - click-jacking
  - django
  - django-nyc-security
  - mozilla
  - phishing
  - security
---
*NB: This is the eighth post in a [series][1] of posts on web application security.*

Click-jacking is a process of &#8220;stealing&#8221; clicks on your site, redirecting them to other places, either by using an [XSS vector][2] to replace the targets of links (or whole sections of the page) or by putting your page in an `iframe` and placing the attacker&#8217;s content *over* yours. The idea is to make this look as seamless as possible, so the user can&#8217;t tell right away that something is wrong.

Click-jacking is a bit of a red-headed stepchild of security vulnerabilities. Usually, there&#8217;s just not a lot of value in it. Where it *is* valuable is ads and phishing schemes.

For ads, if someone can frame your site, place their own ads over yours, and keep people from noticing, then all your ad revenue goes to them. Literally stealing.

Worse, if someone frames your site and puts login controls directly over yours, then even tools like [SiteKey][3] (which [doesn&#8217;t work very well, anyway][4]) won&#8217;t prevent them from collecting username and password data.

This is an [arms race][5] of frame-busting tools and anti-frame-busting tools, but now browsers are helping.

The [X-Frame-Options][6] header tells the browser what, if anything, is allowed to frame the current content. The interesting values are `DENY` and `SAMEORIGIN`. For most use cases, you probably want to send `DENY` on every response, or maybe `SAMEORIGIN`. Just to be on the safe side.

Django [has tools for this][7] since 1.4. Use them.

`X-Frame-Options` isn&#8217;t perfect, especially because it doesn&#8217;t help users with old browsers. Maybe that&#8217;s OK, given your browser support matrix, or maybe it&#8217;s just a starting point, but it&#8217;s a good starting point. Another lock on your door.

## Social Engineering

Especially when it comes to the phishing side of this, we enter the realm of social engineering. In our dream world, users look at the location bar in the browser before entering any important info&mdash;or at least they always use a bookmark or other consistent, hard-coded access method.

But in the real world, there are domains like `twiiter.com`, and Unicode domains that use characters that look the same, and favicons that are locks. And users barely even look at the location bar, anyway.

I dearly wish I knew how to fix that. Browsers are trying, by highlighting the domain in the location bar, by moving favicons out of it (though moving things around probably doesn&#8217;t help). Unfortunately, social engineering will probably always be one of the easiest and most common ways of exploiting any system you can invent.

<p style="text-align:center">
  <a href="http://xkcd.com/538/" title="Actual actual reality: nobody cares about his secrets.  (Also, I would be hard-pressed to find that wrench for $5.)"><img src="http://imgs.xkcd.com/comics/security.png" alt="&#39;Social&#39; Engineering" /></a>
</p>

Maybe browsers can do more? There is already the [Safe Browsing API][8] that Google provides, and many browsers implement, though that relies on use reports and is largely reactive. Maybe user agents can start looking at domain names and if it seems too similar to a domain you regularly visit, it can pop up a warning of its own?

What else do you think browsers could do to help?

  * Go to the [series index][9].
  * Go to the previous post in the series, [Server Configuration][10].
  * Go to the next post in the series, [Stay Up to Date][11].

 [1]: http://coffeeonthekeyboard.com/best-basic-security-practices-especially-with-django-697/
 [2]: http://coffeeonthekeyboard.com/xss-cross-site-scripting-basic-security-part-2-711/ "XSS: Cross-Site Scripting – Basic Security Part 2"
 [3]: http://en.wikipedia.org/wiki/SiteKey
 [4]: http://kottke.org/07/04/sitekey-sucks
 [5]: http://en.wikipedia.org/wiki/Framekiller
 [6]: https://developer.mozilla.org/en/The_X-FRAME-OPTIONS_response_header
 [7]: https://docs.djangoproject.com/en/dev/ref/clickjacking/#setting-x-frame-options-for-all-responses
 [8]: https://developers.google.com/safe-browsing/
 [9]: http://coffeeonthekeyboard.com/best-basic-security-practices-especially-with-django-697/ "Best Basic Security Practices (Especially with Django)"
 [10]: http://coffeeonthekeyboard.com/server-configuration-basic-security-part-7-816/ "Server Configuration – Basic Security Part 7"
 [11]: http://coffeeonthekeyboard.com/stay-up-to-date-basic-security-part-9-834/