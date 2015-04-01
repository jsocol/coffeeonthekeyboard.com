---
title: 'Acronyms you should know: MTTD and MTTR'
author: James
layout: post
permalink: /acronyms-you-should-know-mttd-and-mttr-597/
bitly_url:
  - http://j0.is/14dZUTZ
bitly_hash:
  - 14dZUTZ
bitly_long_url:
  - http://coffeeonthekeyboard.com/acronyms-you-should-know-mttd-and-mttr-597/
dsq_thread_id:
  - 1746519308
categories:
  - Articles
tags:
  - continuous deployment
  - mozilla
  - python
  - sumo
  - webdev
---
If you&#8217;re a [SUMO][1] contributor, there are two acronyms you will start to hear more often from us developers: **MTTD** and **MTTR**.

They mean &#8220;**mean time to detect**&#8221; and &#8220;**mean time to resolve**,&#8221; respectively, and they refer to how long it takes to **detect** an issue in production, and how long it takes to **resolve** that issue once it&#8217;s detected.

As we move toward **[continuous deployment][2]**, these are two of the metrics we&#8217;ll be using to gauge the effectiveness of our tools and processes.

For major production issues, **our MTTR is actually fairly good** right now—if it&#8217;s something that cannot wait until the next scheduled release, it takes us 60-90 minutes from becoming aware of an issue to pushing a fix. I think we can do better with better release processes, but we&#8217;re starting off pretty good and going to **get better**, which is great.

Our **MTTD**, on the other hand, needs work. [SUMO 2.8.1][3] upgraded [Django][4] and included a sweeping change to our CSRF protection—this necessarily affected every form on the site. We discovered three related issues that warranted immediate hotfixes, but we didn&#8217;t discover two of them for **almost two days** when our contributors brought them to our attention.

It&#8217;s **great** that our contributors pointed out these issues to us. Our **community is a critical part** of &#8220;detection&#8221; and I want to encourage everyone to point out issues in the [forums][5] or [IRC][6]. It&#8217;s extremely helpful!

But there are things we can do, too, to **notice things faster**. One thing we are working to add is [**business metric graphs**][7]. We have useful data in [Ganglia][8] right now, but we will be using [Graphite][9] and [Etsy][10]&#8216;s [StatsD][11] to peer into **what our users are doing**. If we deploy a change and notice that no one is [previewing articles][12], for example, we know immediately that we have an issue and can **start diagnosing and fixing it**.

If you follow SUMO development, you&#8217;ll hear us start using terms like MTTD, MTTR, &#8220;detection,&#8221; more, and talking about how to reduce them. We welcome your input and ideas as we start working on these challenges. And of course, keep telling us when things are broken!

 [1]: https://support.mozilla.com/
 [2]: http://coffeeonthekeyboard.com/the-future-of-sumo-development-511/
 [3]: http://moxie.jamessocol.com/bugstats/sumo/2.8.1
 [4]: http://www.djangoproject.com/
 [5]: https://support.mozilla.com/forums/contributors
 [6]: irc://irc.mozilla.org/sumodev
 [7]: http://codeascraft.etsy.com/2010/12/08/track-every-release/
 [8]: http://ganglia.sourceforge.net/
 [9]: http://graphite.wikidot.com/
 [10]: http://codeascraft.etsy.com/
 [11]: https://github.com/etsy/statsd
 [12]: https://bugzilla.mozilla.org/show_bug.cgi?id=654827