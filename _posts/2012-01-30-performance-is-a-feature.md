---
title: Performance is a Feature
author: James
layout: post
permalink: /performance-is-a-feature-623/
bitly_url:
  - http://j0.is/16FwKCf
bitly_hash:
  - 16FwKCf
bitly_long_url:
  - http://coffeeonthekeyboard.com/performance-is-a-feature-623/
categories:
  - Articles
tags:
  - mozilla
  - performance
---
What do I mean when I say &#8220;*performance  is a feature*?&#8221;

For a long time, I got this wrong. When I explained myself, I&#8217;d say that performance was as important as any other feature and worth spending as much time on as any other feature, and you shouldn&#8217;t trade it lightly, like you wouldn&#8217;t trade any other feature lightly.

The thing is, especially on a small team, you might not come back to any particular feature for a few months. So, would this mean you only come back to performance every few months?

Thanks to [Mike Brittain][1] at [Etsy][2], I&#8217;ve figured out just how wrong I was.

*Performance is a feature*, and just like any other feature, it must be *continuously monitored and tested*. What happens when a test breaks or a regression is found in any other feature? Regressions are usually considered top-priority bugs. The same must be true for performance regressions. Just because your small development team isn&#8217;t focusing on a particular feature at the moment doesn&#8217;t mean it&#8217;s OK to break it.

Performance testing isn&#8217;t exactly like most feature testing, but that doesn&#8217;t matter. One of my favorite statements from a QA lead boiled down to &#8220;don&#8217;t get hung up on the tool, focus on what you&#8217;re assuring.&#8221; Use the tools—like [graphs][3], external and [on-site performance monitoring][4]—you need to use to make sure performance doesn&#8217;t regress.

 [1]: https://twitter.com/mikebrittain
 [2]: http://codeascraft.etsy.com/
 [3]: https://github.com/jsocol/pystatsd
 [4]: https://github.com/andymckay/django-statsd