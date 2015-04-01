---
title: Surviving Pac Man
author: James
layout: post
permalink: /surviving-pac-man-422/
bitly_url:
  - http://j0.is/9enDiY
bitly_hash:
  - 9enDiY
bitly_long_url:
  - http://coffeeonthekeyboard.com/surviving-pac-man-422/
dsq_thread_id:
  - 1746519518
categories:
  - Articles
tags:
  - Database
  - ddos
  - mozilla
  - pac man
  - spike
  - sumo
  - traffic
---
On Friday, Google showed off a fun new [doodle][1] in honor of the 30th anniversary of Pac Man: a [Pac Man clone][2], complete with sounds.

Unfortunately, in the initial release, those sounds started playing automatically—an oversight or an homage to `<bgsound>`, I guess. Even if Google was open in a background tab or window, or in a hidden iframe created by an [add-on][3], the Pac Man music and sound effects would start.

And that [confused some people][4].

Many people came to [SUMO][5] looking for an explanation, and many of them, not finding anything in the knowledge base, started posting to our forum. So many, in fact, that our database server started running out of connections.

The pounding we took on the forums also caused replication on our slave databases to fall behind by as much as 1.25 hours, so even when we wrote an article about the noises [article has been removed], it didn&#8217;t show up for most people.

As Sean put it: &#8220;We just got DDOSed by Pac Man.&#8221;

To shore up the site and bring it back from the brink of toppling over, we worked with IT (thanks, Dave!) to implement a number of temporary solutions. We&#8230;

  * &#8230;disabled a particular kind of slow, frequent, and useless query.*
  * &#8230;blocked Google&#8217;s crawler from indexing the site.
  * &#8230;disabled our own sumobot&#8217;s forum-crawling features.
  * &#8230;rotated DB slaves out of the production pool to allow them to catch up.

Google has already removed the Pac Man doodle from their home page, and we can revert most of the emergency measures here on Monday. But the event does remind us to look at what we&#8217;re doing in Kitsune, our rewrite, to weather storms like this in the future.

One idea, suggested by [Dave Dash][6], is a read-only mode where all pages that can trigger database writes are temporarily disabled. We&#8217;ll be looking pretty seriously at this over the next couple of days.

Another important take-away is to make damn sure pages only trigger database writes if they really need to. Writes can never bounce off a cache, so they are very expensive.

Finally, we should be more proactive in how we interact with our Zeus cache. We&#8217;ll also think about whether it makes sense to start using [Wil Clouser&#8217;s][7] Zeus interface, [Hera][8], sooner than later.

&#8220;Too much traffic&#8221; is the best problem a web development team can have. Hopefully, the first time this happens to Kitsune, we&#8217;ll be ready.

* The queries that increment the number of views a forum thread has gotten are particularly slow for some reason. They&#8217;re also wildly inaccurate, since most people see a cached version of those pages and never trigger the query. The worst part: they occur on every (non-cached) page view, even while just reading.

(This post was [translated into Belorussian][9], isn&#8217;t that cool?)

 [1]: http://www.google.com/logos/
 [2]: http://www.google.com/pacman/
 [3]: https://addons.mozilla.org/en-US/firefox/addon/2207/
 [4]: http://support.mozilla.com/en-US/forum/1/678028
 [5]: http://support.mozilla.com/
 [6]: http://davedash.com/
 [7]: http://micropipes.com/blog/
 [8]: http://github.com/clouserw/hera
 [9]: http://pc.de/pages/surviving-pac-man-be