---
title: s.hort.cc makes s.hort.cuts
author: James
layout: post
permalink: /shortcc-makes-shortcuts-84/
bitly_url:
  - http://j0.is/14dZWv4
bitly_hash:
  - 14dZWv4
bitly_long_url:
  - http://coffeeonthekeyboard.com/shortcc-makes-shortcuts-84/
tags:
  - Projects
  - short
  - Social Networking
  - url
  - Web 2.0
  - web service
---
Yet Another Short URL.

I just launched [s.hort.cc][1] to offer something that TinyURL and Tiny.cc didn&#8217;t: an easy API for creating short URLs, or &#8220;s.hort.cuts,&#8221; and returning them in a format my program could use.

You can visit the [s.hort.cc ][1]website, of course, but you can also create a short URL and have it returned to you in XML, JSON, YAML, or plain text. (Even if you decide to parse the whole XHTML page, it&#8217;s easy to find the s.hort.cut with the DOM or an XPath parser.) I&#8217;ll gladly add other formats if people suggest them.

I&#8217;m working on a Firefox extension, but in the meantime, drag this link: [s.hort.cut][2] to your bookmarks toolbar to automagically turn the current page into a s.hort.cut.

Read more technical info below the break.<!--more-->

### Technical Things!

To use the API, you just need to send a request to `http://s.hort.cc/create.php`, via GET or POST, with two parameters:

  * `url` — contains the URL-escaped long URL you want to shrink, and
  * `format` — one of &#8220;`json`&#8220;, &#8220;`xml`&#8220;, &#8220;`yaml`&#8220;, or &#8220;`text`&#8221; or &#8220;`plain`&#8220;, depending on what you need.

s.hort.cc returns the new URL in the correct format and with the correct Content-type heading. (For JSON, the content is available both in the response body and in the X-JSON header.)

If possible, please try to set a unique and meaningful User-Agent header, just so I can track usage, and so your app won&#8217;t be subject to rate limiting because of someone else&#8217;s problem.

Not that there *is* rate limiting. You can use the API as much as you want. But if you&#8217;re going to access it more than 60 times per minute, or once per second on average, please let me know.

On the server-side, s.hort.cc is written in [PHP][3] and backed by [MySQL][4] and [Memcached][5] to speed up requests and reduce server load.

 [1]: http://s.hort.cc/
 [2]: javascript:void(window.open('http://s.hort.cc/create.php?url='+escape(window.location), 'shortcut', 'status=0,toolbar=0,location=0,menubar=0,scrollbars=0,height=320,width=400')); "s.hort.cut"
 [3]: http://www.php.net/
 [4]: http://www.mysql.com/
 [5]: http://danga.com/memcached/