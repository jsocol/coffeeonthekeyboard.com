---
title: 'No Checks: Cache!'
author: James
layout: post
permalink: /no-checks-cache-44/
blogger_blog:
  - urbaneexistence.blogspot.com
blogger_author:
  - James
blogger_permalink:
  - /2007/06/no-checks-cache.php
bitly_url:
  - http://j0.is/14dZFZa
bitly_hash:
  - 14dZFZa
bitly_long_url:
  - http://coffeeonthekeyboard.com/no-checks-cache-44/
categories:
  - Articles
---
The title is a horrible pun, but doesn&#8217;t doesn&#8217;t mean Internet Explorer 7 doesn&#8217;t ignore web designers and caches things it shouldn&#8217;t.

I&#8217;m heading development of a small script to let users upload and vote on pictures of China, and at the end of the voting period, we take the most popular pictures and turn them into a book. The "vote" action seemed like the perfect candidate for an AJAX implementation: we didn&#8217;t want a page reload, and all it does is increment a value in a database row. Easy enough.

Or not. While it worked out of the box in Firefox and IE 6, we discovered an interesting problem with IE 7&#8217;s "native" implementation of the XMLHttpRequest object. IE caches the first returned value and doesn&#8217;t actually do the server call again. You actually have to quit the browser and reopen the page to repeat the action and get a new result.

Our project limits voting to once every 10 seconds, and if you vote faster than that, it asks you to wait. If it&#8217;s been 10 seconds, it thanks you for your vote.

In IE, it counted the first vote, and then kept saying thank you, without counting subsequent votes. A quick search revealed that most people solve this by appending some junk data to the query string, a random number or timestamp, that changes all the time. A solution my boss and I had also decided to try, to no avail.

In the end, it was a combination of junk data and four separate "no-cache" headers, telling IE in every way possible that the data was uncacheable and, among other things, had expired in 1997, that finally worked.

Microsoft, and the internet in general, would benefit from *actually* complying to standards.