---
title: 'Password Storage &#8211; Basic Security Part 1'
author: James
layout: post
permalink: /password-storage-basic-security-part-1-706/
bitly_url:
  - http://j0.is/16FvwXA
bitly_hash:
  - 16FvwXA
bitly_long_url:
  - http://coffeeonthekeyboard.com/password-storage-basic-security-part-1-706/
dsq_thread_id:
  - 1746519071
categories:
  - Security
tags:
  - django
  - django-nyc-security
  - mozilla
  - passwords
  - security
---
*NB-1: This is the first post in a [series][1] of posts on web application security.*

*NB-2: [Fred][2] wrote a [great post on password storage][3]. You should read it.*

I&#8217;m assuming we&#8217;re talking about web apps, and most web apps have user accounts, and most of those have passwords. That is: I&#8217;m assuming *you&#8217;re storing passwords*.

Password storage is in the news pretty constantly because people keep doing it wrong, storing passwords&#8230;

  * &#8230;in plain text.
  * &#8230;with simple MD5 hashes.
  * &#8230;or using any fast hashing function.
  * &#8230;unsalted.

Fortunately, The Right Thing&trade; is easy. (Unfortunately, many of the apps you use every day are doing The Wrong Thing&trade; and you just don&#8217;t know it yet.)

Why is it important to do The Right Thing&trade; even if you&#8217;re not running a big app? Because users do dumb things. They reuse passwords, including the passwords to their email address and bank accounts. You should assume that at some point, someone will have your user database, and if they can easily extract an email address and a password, they&#8217;ve basically got a user&#8217;s entire identity.

So what is The Right Thing&trade;?

  * Use a cryptographically slow hash function. [bcrypt][4] and [PBKDF2][5] are excellent choices. Even if someone has complete access to your database, it&#8217;ll take a long time (longer for longer passwords) to calculate these, and as processors get faster, they both have a *work factor* that can be increased, so the time to generate each hash can remain constant.
  * Use [HMAC][6] with expirable keys stored on the filesystem (or anywhere outside the user database). This way, even if the users get the database (which often happens via a SQL-injection vector) they still *also* need to compromise the file system or settings information.

Libraries like [django-sha2][7] will do all of this for you. (Ignore the name, it&#8217;s a historical artifact: it uses bcrypt and HMAC.) They exist for most frameworks, so you shouldn&#8217;t have to roll your own.

Which is a final thought: unless you have a Ph.D in this, **never roll your own crypto**. And even then, you probably want something tested by the community and other experts.

  * Go to the [series index][8].
  * Go to the next post in the series, [XSS: Cross-Site Scripting][9].

 [1]: http://coffeeonthekeyboard.com/best-basic-security-practices-especially-with-django-697/
 [2]: http://fredericiana.com/
 [3]: https://blog.mozilla.org/webdev/2012/06/08/lets-talk-about-password-storage/
 [4]: http://en.wikipedia.org/wiki/Bcrypt
 [5]: http://en.wikipedia.org/wiki/PBKDF2
 [6]: http://en.wikipedia.org/wiki/HMAC
 [7]: https://github.com/fwenzel/django-sha2
 [8]: http://coffeeonthekeyboard.com/best-basic-security-practices-especially-with-django-697/ "Best Basic Security Practices (Especially with Django)"
 [9]: http://coffeeonthekeyboard.com/xss-cross-site-scripting-basic-security-part-2-711/ "XSS: Cross-Site Scripting – Basic Security Part 2"