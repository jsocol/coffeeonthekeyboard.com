---
title: What Time Is It There?
author: James
layout: post
permalink: /what-time-is-it-there-273/
bitly_url:
  - http://j0.is/fyGYX
bitly_hash:
  - fyGYX
bitly_long_url:
  - http://coffeeonthekeyboard.com/what-time-is-it-there-273/
categories:
  - Articles
---
[We][1] just pushed an update to [What Time Is It There?][2], so I thought I&#8217;d take the time to mention it here, since I&#8217;ve already done so on [Twitter][3]. You can now [link to a time][4].<!--more-->

What Time Is It There?, or WTIIT, is a quick, visual reference card for US time zones. It&#8217;s a collaborative effort of [Ben Lew][5] and I. And when I say &#8220;collaboration,&#8221; I mean it was 100% Ben&#8217;s idea, and I refactored some JavaScript for him.

Ben&#8217;s original version showed you the current time in six time zones. [My first update][6] completely rewrote the JavaScript and added the ability to specify a time, say &#8220;at 9:15pm eastern&#8221; or &#8220;in 2 hours 30 minutes&#8221;. Let&#8217;s call that version 1.0, release to the wild.

Rather quickly I noticed a bug causing problems with when &#8220;PM&#8221; was capitalized and there was no space before it (eg: &#8220;at 5PM&#8221; instead of &#8220;at 4pm&#8221; or &#8220;at 1 PM&#8221;). So I sent him a patched version that also changed the behavior when no &#8220;am&#8221; or &#8220;pm&#8221; was given. Version 1.0.1 assumed that hours between 12 and 6 were in the afternoon unless you told it otherwise, while 7-11 are assumed to be morning.

Last night I sent Ben a new patch, version 1.0.2, that adds what I think is an important new feature: you can now [link to a time][4].

<img class="aligncenter size-full wp-image-274" title="What Time Is It There? lets you link to a time." src="http://coffeeonthekeyboard.com/wp-content/uploads/2009/08/wtiit-1.0.2.gif" alt="What Time Is It There? lets you link to a time." width="545" height="139" />

When you enter a time and hit enter or click outside the box, the URL is updated with that time. If you copy the URL, you can save or send it and when you return to that URL, the page will start off with whatever time you saved.

WTIIT 1.0.2 includes two additional changes. &#8220;a&#8221; and &#8220;p&#8221; now work if you don&#8217;t like typing the &#8220;m&#8221;, and—finally!—the pesky 12 noon/midnight bug is squashed. Previous behavior with 12 o&#8217;clock was, shall we say, wonky at best, and often plain wrong. (It could literally display the opposite of what you typed.) Now, [it parses noon and midnight correctly][7].

Just thought I&#8217;d share.

 [1]: http://twitter.com/n0s0ap
 [2]: http://whattimeisitthere.info/
 [3]: http://twitter.com/jamessocol
 [4]: http://whattimeisitthere.info/#at-1-pacific
 [5]: http://benlew.com/
 [6]: http://benlew.com/blog/2009/07/what-time-is-it-there-update/
 [7]: http://whattimeisitthere.info/#at-12-mountain