---
title: IE8 and Version Targeting
author: James
layout: post
permalink: /ie8-and-version-targeting-70/
blogger_blog:
  - urbaneexistence.blogspot.com
blogger_author:
  - James
blogger_permalink:
  - /2008/03/ie8-and-version-targeting.php
bitly_url:
  - http://j0.is/14dZAoj
bitly_hash:
  - 14dZAoj
bitly_long_url:
  - http://coffeeonthekeyboard.com/ie8-and-version-targeting-70/
categories:
  - CSS
  - Standards
tags:
  - Browsers
  - CSS
  - IE8
  - Standards
  - version targeting
---
Two months after the whole of the internet has had their say, I thought I&#8217;d throw some new kindling on the fire of Internet Explorer 8&#8217;s version-targeting mechanism. It&#8217;s crap.

The key issue is the default behavior: if I never change my server configuration or every page on my site, they will &#8220;forever&#8221; be locked in IE7 mode. This is a blow to the heart of the idea of progressive enhancement, or even graceful degradation, and will certainly not encourage developers to make their sites IE8—and thus Acid2—compatible.

Why worry about learning the rules when you have a broken version &#8220;forever?&#8221;

And what of this &#8220;forever?&#8221; How long can Microsoft reasonably include *every previous version of IE* in their new releases? Five years? Say to IE 9? 10 years to IE 10 or 11? At that point there will be 5 separate rendering engines, IE 6 and up, embedded in that increasingly large, increasingly slow program.

Of course, there is also the issue of implementation: Microsoft has said unto us that this shall be. If they really want to get on the standards bandwagon, shouldn&#8217;t this have been brought to the W3C, at least for advice?

I have a much more radical suggestion they may not like. Microsoft should abandon &#8220;Internet Explorer.&#8221; Not the product, but the name, and specifically the abbreviation &#8220;MSIE&#8221; in the browser string.

They&#8217;ll also need to dump the `window.ActiveXObject` class, perhaps replacing it with a `window.ActiveXControl` or `window.AXObject` class. These are the most common ways of identifying IE. If IE shows up like any other standards-compliant browser, there should be no problems for older pages.

I tried to find something good to say about this, but I can&#8217;t. It&#8217;s a bad idea from the bottom up. Unfortunately, we&#8217;re stuck with it.

So I will take Microsoft&#8217;s built-in cheat—a not-so-tacit admission that this idea is not viable in the long-term—and adjust my server to send `IE=edge` with every page. That way I get to keep the progressive enhancement that has served me so well.