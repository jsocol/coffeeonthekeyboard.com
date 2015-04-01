---
title: Irregular Update, 24/Oct/2014
author: James
layout: post
permalink: /irregular-update-24oct2014-1176/
pw_single_layout:
  - 1
dsq_thread_id:
  - 3152976681
categories:
  - Articles
tags:
  - api
  - Front-end
  - irregular update
  - javascript
  - react
  - weekly update
---
Another update! Wow!

The past two weeks have all been on [TodaysMeet Teacher Tools][1] except for the afternoon I spent dealing with a hurt finger (not exactly *badly* hurt but I still can&#8217;t really use my right index finger to type, it&#8217;s annoying). And at some point in there I shipped updates for [POODLE (CVE-2014-3566)][2].

Teacher Tools are *available for pre-order* which means I finished the sixth, implicit component: credit card forms built with Stripe. Even when you can offload the PCI-compliance to a provider there&#8217;s still quite a bit of record keeping, and interacting with user interaction, JavaScript callbacks, [webhooks][3], and more made keeping track of everything a bit of a mental challenge.

And more exciting: all the heavy lifting for Teacher Tools is *done*! Mute, Topics, My School, Pause, and Transcripts are all done but for a very small list of polish tasks and, of course, testing.

Most recently—read: today—I finished the new signed-in home page, which includes closed rooms. The old version, because it only showed open rooms and could assume that list was *probably* short-enough, was static, rendered on the server, and sorted by the browser. The home page is an API client application built on top of [React][4] and a fairly simple new endpoint that let me add a room name search. Sneak peak!

<img src="http://coffeeonthekeyboard.com/wp-content/uploads/2014/10/Screenshot-2014-10-24-16.54.43.png" alt="Screenshot 2014-10-24 16.54.43" width="782" height="333" class="aligncenter size-full wp-image-1177" />

(I know I can—and will try to—do better visually but the functionality is all there and I would feel OK starting to roll this out.)

Huge thank-yous to [G Scott Olson][5] and [zpao][6] for helping me out as I was starting out earlier this week. I&#8217;m a fan and am working on writing up the &#8220;Rewriting TodaysMeet with React&#8221; blog post.

I expect React is going to end up in a lot more of the site.

It&#8217;s going to be a good weekend to do some polishing and start replacing the word &#8220;pre-order&#8221; with &#8220;upgrade&#8221;!

 [1]: https://todaysmeet.com/about/teachertools
 [2]: https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2014-3566
 [3]: http://coffeeonthekeyboard.com/best-practices-for-happy-webhooks-1138/
 [4]: http://facebook.github.io/react/
 [5]: https://twitter.com/gscottolson
 [6]: https://twitter.com/zpao