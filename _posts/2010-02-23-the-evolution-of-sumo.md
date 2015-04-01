---
title: The Evolution of SUMO
author: James
layout: post
permalink: /the-evolution-of-sumo-339/
bitly_url:
  - http://j0.is/97iCKq
bitly_hash:
  - 97iCKq
bitly_long_url:
  - http://coffeeonthekeyboard.com/the-evolution-of-sumo-339/
dsq_thread_id:
  - 1746519383
categories:
  - Articles
tags:
  - django
  - kitsune
  - mozilla
  - PHP
  - python
  - sumo
  - web development
---
When I joined the <a title="SUpport dot MOzilla dot com" href="http://support.mozilla.com/" target="_blank">SUMO</a> team six months ago, the team was just starting a discussion of &#8220;where do we go from here?&#8221;  SUMO was built on a CMS called <a href="http://tikiwiki.org/" target="_blank">TikiWiki</a>, and had diverged pretty significantly in two years. (David Tenser wrote <a href="http://blog.mozilla.com/sumo/2010/02/18/the-bright-future-of-the-sumo-platform/" target="_blank">a more detailed history</a> if you&#8217;re interested.)

After a few months of talking and testing—and a few changes of direction—we&#8217;ve decided that SUMO will follow our colleagues on <a title="Addons dot Mozilla dor Org" href="https://addons.mozilla.org/" target="_blank">AMO</a> and move to a custom web application, built on [Django][1], a development framework in Python.

Why are we committing to such a dramatic new direction? Three major reasons. <!--more-->

*Keep in mind that SUMO was built on TikiWiki 1.10, a little more than two years out of date.*

### Performance

TikiWiki is a very feature-rich application. An unfortunate trade-off for us is performance, especially on a site serving 16 million users every week. <a href="https://bugzilla.mozilla.org/show_bug.cgi?id=532498" target="_blank">As our European users</a> in particular know, SUMO can be unacceptably slow at times, especially when editing articles. Many of the changes we made to the platform—most of which were contributed back over the past few months—were to improve performance via tools like output caching, database replication, and just refactoring. When we evaluated the latest version of TikiWiki, we found that performance was around the same, on average.

In the new platform, we&#8217;ll be taking advantage of techniques now available, including query and template/fragment caching and expect to see dramatic performance improvements. We&#8217;ll also be avoiding some of the performance pitfalls that TikiWiki fell into over the years with improvements to the security, database, and templating layers, among others.

But the biggest performance impact—I expect—will be moving from a general-purpose CMS to a dedicated web application, focused on providing the SUMO experience.

### Hackability

To work on SUMO, you have to overcome a steep learning curve. Components tend to be tightly-coupled, or grouped in unintuitive ways, and are not as extensible as we&#8217;d like. The lack of a comprehensive test suite leaves changes to important sections of code open to introducing regressions in otherwise unrelated, dependent areas. SUMO 1.x also fails to function without a relatively complete copy of its database, which makes it difficult for community members outside the company to contribute.

With the new platform, and some discipline from the team, our goal is to improve all of these and make it easier for someone to get started hacking on SUMO.

  * We&#8217;ll be striving to keep code loosely-coupled and extensible—including using existing or external libraries whenever possible, and turning our own contributions into external libraries where possible.
  * We&#8217;re adopting a test-driven development workflow to ensure that our components are easier to safely hack, and lighten the load on our QA team by reducing regressions.
  * TDD and Django will make it easier to work without a copy of the database, using fixtures and migrations to minimize the dependence on real data.

The net effect of these decisions will be to lower the barrier to entry to SUMO development, and hopefully make useful code available to other projects. Wil Clouser <a href="http://micropipes.com/blog/2009/11/17/amo-development-changes-in-2010/" target="_blank">listed more strengths</a> of Django as a platform when the AMO team decided to switch.

### Strength in Numbers

By using the same platform as AMO, both teams will benefit from sharing code and resources. We&#8217;re already using the same template adapter, database router, caching layer, and HTML sanitizer. As open source developers often say: &#8220;with enough eyes, all bugs are shallow,&#8221; and by sharing code we get more eyes on it. We&#8217;ll benefit from insights the AMO team has gleaned by starting the process of moving from a PHP framework to Python just ahead of us. We&#8217;ll even be able to send code reviews across teams and benefit from deeper knowledge of the various problem domains we share: have a question about localization? Both teams can share expertise and best-practices.

Solving problems once and sharing the solution directly reduces the amount of work both teams have to do. And when SUMO writes code in such a way that AMO can use it, we can also release it separately so others can benefit from our solutions—and point out flaws and contribute improvements.

### Other Changes

Also among the changes coming in the next year:

  * **Version Control System. **Though we don&#8217;t have a specific plan in place, it seems likely that SUMO will be moving from SVN to Git for source control. Because Git is distributed, it allows us to use a more <a href="http://nvie.com/archives/323" target="_blank">collaborative workflow</a>, and it&#8217;s easier for us to push our code to public repositories like [Github][2].
  * **Continuous Integration.** We&#8217;ll be using [Hudson][3] for continuous integration, which will automate our tests and alert us to potential issues and regressions. The web QA team has also been working to make sure our [Selenium][4] tests can run through Hudson, greatly increasing test coverage for a web application like SUMO.
  * **Interface Localization**. One of the ways we plan to improve the SUMO experience this year is by moving our interface localization to [gettext][5], which is an industry-standard tool for localization. As we move parts of the site from TikiWiki to Django, those new sections will be localized via gettext, which helps us take advantage of our great community with tools like [Verbatim][6].

### A Foundation for the Future

The goal of all of this work—and it will be a lot of work—is to put SUMO on a solid foundation for future growth and, at the same time, improve the experience for everyone—from developers to contributors to localizers to visitors. We have a daunting and aggressive road ahead of us, but I&#8217;m confident that we&#8217;ll emerge in a better place.

SUMO 2 is codenamed [Kitsune][7], and is already [up on Github][8].

 [1]: http://www.djangoproject.com/
 [2]: http://github.com/
 [3]: https://hudson.dev.java.net/
 [4]: http://seleniumhq.org/
 [5]: http://www.gnu.org/software/gettext/
 [6]: http://localize.mozilla.org/
 [7]: https://wiki.mozilla.org/Support/Kitsune
 [8]: http://github.com/jsocol/kitsune