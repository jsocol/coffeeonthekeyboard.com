---
title: The future of SUMO development
author: James
layout: post
permalink: /the-future-of-sumo-development-511/
bitly_url:
  - http://j0.is/14dZxJl
bitly_hash:
  - 14dZxJl
bitly_long_url:
  - http://coffeeonthekeyboard.com/the-future-of-sumo-development-511/
dsq_thread_id:
  - 1746519658
categories:
  - Articles
tags:
  - kitsune
  - mozilla
  - sumo
---
Just this month, the [SUMO][1] development team completed our transition to our new platform, [Kitsune][2]. This small release represented the culmination of nearly a year of great work, and I couldn&#8217;t be prouder of the team.

At first, as the end of the tunnel approached, and we weren&#8217;t sure what we&#8217;d be doing next, I felt a little like Inigo Montoya: I&#8217;d been in the rewrite business so long, now that it&#8217;s over&#8230;

2010 was a year of investment in our code, and in our infrastructure, both hardware and software. We moved our code from svn to [git][2]; started using Hudson to run our comprehensive [unit test suite][3]; centralized our configuration to simplify deployment. Now it&#8217;s time to start taking advantage of that investment.

In that spirit, I want to issue a John Kennedy-style challenge: **by this time next year, I want SUMO to [deploy][4] [continuously][5]**.

There is a lot of work to do to get there.

  * **Pushing code to production needs to be automated**, for all but the biggest, downtime-requiring changes.
  * We need to **expand automated test coverage** to include our **front-end and JavaScript** code.
  * Our code reviews and **standards** will need to be **even higher**.
  * We&#8217;ll need to **redefine the relationship between development and QA**.
  * When there are problems, we need the **agility to respond quickly** and the focus to [learn from them][6] and improve.
  * We&#8217;ll need to be able to **[dark launch][7] and [flip on features][8]**, preferably with the flexibility to test with small groups.
  * We&#8217;ll need to **reevaluate our branch management** and **staging environments**.
  * We&#8217;ll need to **rethink how we organize and prioritize work**.

And unlike 2010, we&#8217;ll have to make these investments and improvements while maintaining a fast-paced development schedule.

This is not something the SUMOdev team can do alone: this is a challenge for us, for our ops team, and for QA as well. I look forward to working more closely with these teams as we chase this target.

Continuous deployment will bring a number of benefits—many of the requirements I just listed are benefits and solid goals by themselves—especially for contributors.

  * Bugs are **fixed for everyone** as soon as they&#8217;re fixed for us.
  * We can **respond to issues** faster.
  * Code will be **tested at production scale**.
  * Our **processes will continually improve**.
  * We&#8217;ll **reduce the load** on individuals from **IT and QA**.

I call this a challenge because it will not be easy: it will be hard. It will push the bounds of our experience and our ingenuity. SUMO will be better, and we will be better.

How do we get there? Frankly, I don&#8217;t know yet. There are some clear, actionable items, like front-end testing and feature flags, and there is some brainstorming to do. There will be unanticipated hurdles to overcome and we will almost certainly make some missteps, and we will have to work around those.

In 2010, we pushed ourselves in terms of *how much* we could get do, and while we accomplished an incredible amount, that&#8217;s not a healthy pace to set for another year. In 2011, I want us to push ourselves in terms of *what* we can do, and *how* we do it.

**2011 will be a great year** for SUMOdev. And with this challenge, I just want to say: **Game On.**

 [1]: http://support.mozilla.com/
 [2]: http://github.com/jsocol/kitsune
 [3]: https://hudson.mozilla.org/job/sumo-master/
 [4]: http://radar.oreilly.com/2009/03/continuous-deployment-5-eas.html
 [5]: http://toni.org/2010/05/19/in-praise-of-continuous-deployment-the-wordpress-com-story/
 [6]: http://www.startuplessonslearned.com/2008/11/five-whys.html
 [7]: http://agiletesting.blogspot.com/2009/07/dark-launching-and-other-lessons-from.html
 [8]: http://code.flickr.com/blog/2009/12/02/flipping-out/