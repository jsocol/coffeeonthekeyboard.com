---
title: So You Wanna Help Mozilla?
author: James
layout: post
permalink: /so-you-wanna-help-mozilla-316/
bitly_url:
  - http://j0.is/90ciMQ
bitly_hash:
  - 90ciMQ
bitly_long_url:
  - http://coffeeonthekeyboard.com/so-you-wanna-help-mozilla-316/
dsq_thread_id:
  - 1746519273
categories:
  - Articles
tags:
  - amo
  - mozilla
  - Projects
  - sumo
  - support
  - tools
  - web dev
  - webdev
  - workflow
---
A common theme we heard in responses to our [web developer survey][1] was: &#8220;I wish I could help Mozilla, but I&#8217;m just a web developer.&#8221;

Well, fellow web ninjas, you *can* put your skills to work with Mozilla and help make the web a better place. Our web projects are open, just like Firefox, and we&#8217;d love your help!

If you&#8217;re a web developer and want to help Mozilla and Firefox users while working on sites that see millions of visitors every day, follow me through the jump and I&#8217;ll show you around our shop and introduce you to the tools we use.<!--more-->

### Our Projects

Mozilla Web Dev is responsible for pretty much every web site at Mozilla, from [Addons][2] to [Support][3] to [Mozilla.com][4] to [Spread Firefox][5]. We also take care of web services like the [Application Update Service][6], [Plugin Finder Service][7] and the [Crash Report Beacon][8].

Almost all of our code (everything except Socorro—the crash report beacon—I think) is available on [svn.mozilla.org][9]. You can browse around the server and see the source right now. It will all basically run on the standard LAMP (where P is PHP or Python) stack.

### Our Tools

The control center of web development at Mozilla is [Bugzilla][10]. All our work takes the form of &#8220;bugs&#8221; here. A &#8220;bug&#8221; is not necessarily a problem with the software. A new feature or a software update would also be called a &#8220;bug.&#8221; I&#8217;ll talk about the life-cycle of a web development bug below.

IRC is just as important as Bugzilla. Both our staff and our contributors are geographically diverse, and IRC gives us a good way to coordinate and talk to each other around the globe. If you&#8217;re new to IRC, [ChatZilla][11] is an easy place to start. Most projects have their own channel on [irc.mozilla.org][12], like [#sumo][13] for Support and [#amo][14] for Addons. We&#8217;re also usually in [#webdev][15].

As web developers, are chief weapon is HTML. HTML and CSS. HTML, CSS, and JavaScript. *No one expects the&#8230;* (Sorry.) On the front-end, we chiefly work in HTML, CSS, and JavaScript, and occasionally other technologies like XML. On the back-end, nearly all of our projects are in PHP or Python.

For version control we mostly use Subversion, though git (and git-svn) has gained a bunch of ground lately. (Addons will be [moving to git][16] sometime next year.)

To work on your own computer, you&#8217;ll probably need to set up Apache, MySQL, and PHP or Python. [XAMPP][17] can be incredibly helpful here. Working on Linux (or Mac OS) is usually easier than Windows, and closer to our server environment. A tool like [VirtualBox][18] will let you run a Linux virtual machine on most other operating systems. It&#8217;s a little slower but switching back and forth is easy. I&#8217;ll try to write about my local development set up soon.

### Our Workflow

Let&#8217;s go over the life of a bug. This is a little long-winded.

A bug is born either when someone describes a problem or when we decide to add a new feature. (Every project has its own way of doing the latter.) Sometimes our Web QA team finds problems, sometimes community members do. Then the bug is created, or filed, usually by the person who discovers it—that could be you!

Once a bug has been filed, possibly after some discussion of the best way to fix it, a developer—and that could be you, too—will &#8220;take&#8221; the bug, accepting responsibility for fixing it. Fixing a bug means changing the application in some way: changing a behavior, adding a localization, etc.

When the developer believes they&#8217;ve fixed the problem in their copy, they generate a patch (Subversion&#8217;s &#8220;diff&#8221; command is useful). They then upload that patch as a new attachment, and request review (r?) from someone else. Who does the review will depend on the project and how busy everyone is. If you&#8217;re not sure who to ask, ask in IRC.

The patch will either get an r+ or an r-. An r+ means it&#8217;s good to go, and it can usually be checked in to Subversion (unless there are reasons to wait). An r- means there&#8217;s something wrong. This can range from a patch containing some extraneous data to a patch not solving the problem, and an r- almost always includes a description of the problem.

When a patch with a positive review (r+) gets checked in to Subversion, the developer will include the revision in the bug and change it from *new* to *resolved: fixed*. In a few minutes, the change will appear on a staging server that&#8217;s updated frequently with the latest code. Our Web QA staff and contributors will test the bug, compare expected to actual behavior, and with either *reopen* the bug, if it&#8217;s not fixed, or set the status to *verified*. Then the bug is considered done. Verified bugs rarely reopen.

Once all the bugs for a milestone are done, we &#8220;freeze&#8221; the source against new commits while Web QA does a final check that everything is in good shape. When QA signs off on the current code, we push the new version onto our production web servers.

So that&#8217;s the life of a bug, from filed to production. When it gets to production, your code can be seen or used by millions of people every day.

### Join Us

So if you&#8217;re a web developer and want to help Mozilla and Firefox, head over to Bugzilla or IRC and get started. We hope to see you soon!

 [1]: http://hacks.mozilla.org/2009/11/web-developer-survey-update/
 [2]: https://addons.mozilla.org/
 [3]: http://support.mozilla.com/
 [4]: http://www.mozilla.com/
 [5]: http://www.spreadfirefox.com/
 [6]: https://wiki.mozilla.org/AUS
 [7]: https://wiki.mozilla.org/PFS2
 [8]: https://wiki.mozilla.org/Socorro
 [9]: http://viewvc.svn.mozilla.org/vc/
 [10]: http://bugzilla.mozilla.org/
 [11]: https://addons.mozilla.org/en-US/firefox/addon/16
 [12]: http://irc.mozilla.org/
 [13]: irc://irc.mozilla.org/sumo
 [14]: irc://irc.mozilla.org/amo
 [15]: irc://irc.mozilla.org/webdev
 [16]: http://micropipes.com/blog/2009/11/17/amo-development-changes-in-2010/
 [17]: http://www.apachefriends.org/en/xampp.html
 [18]: http://www.virtualbox.org/