---
title: Weekly(?) Update for 5/4/2010
author: James
layout: post
permalink: /weekly-update-for-542010-387/
bitly_url:
  - http://j0.is/15WnEQx
bitly_hash:
  - 15WnEQx
bitly_long_url:
  - http://coffeeonthekeyboard.com/weekly-update-for-542010-387/
dsq_thread_id:
  - 1746519266
categories:
  - Articles
---
Attempt #2 at a weekly update.

**Last week:**

  * Stopped work, for now at least, on [bug 532498][1]. It had gone way over budget and there was no end in sight. Feel free to ping me for details.
  * Got the [locale code into Kitsune][2]. ([bug 549013][3])
  * Got [support for custom word lists ][4] in. ([bug 550103][5])
  * Got the machinery built to do [Sphinx][6] tests in [bug 554740][7]. All that&#8217;s left is figuring out how best to clean up after myself.
  * Fixed a bug in [Wil Clouser&#8217;s Tower L10n library][8] for [bug 556810][9] and got a first draft up.
  * Started work on pulling [Dave Dash&#8217;s minify app][10] out of [Zamboni][11] for [bug 550515][12].
  * Started reviewing [bug 553131][13] (working with Tiki sessions in Kitsune).

**This week**:

  * Finish out 556810 (string extraction) and 554740 (Sphinx tests and category exclusion).
  * Finish up library extraction for 550515 (concat and minify JS and CSS).
  * Get [Hudson][14] running our test suite. ([bug 556449][15])
  * Fix up [FlatQS][16]. ([bug 554206][17])
  * Start [1.5.4][18] work and come up with release documentation. (!!!)
  * Work with L10n community to figure out what we need to get over that way.
  * [Reviews][13], [reviews][19], [reviews][20]. (I hope.)

**Stretch goals:**

  * Get some better error handling when the Sphinx server is down. ([bug 554778][21])
  * See if we can&#8217;t get the outermost templates working with RTL ([bug 437891][22]).

 [1]: https://bugzilla.mozilla.org/show_bug.cgi?id=532498
 [2]: http://github.com/jsocol/kitsune/commit/fbc0f6951d4bc618886baf1556ca3f87ac3a2142
 [3]: https://bugzilla.mozilla.org/show_bug.cgi?id=549013
 [4]: http://github.com/jsocol/kitsune/commit/60de203050d163d789fd7af33a2702b8ccd0c4f5
 [5]: https://bugzilla.mozilla.org/show_bug.cgi?id=550103
 [6]: http://www.sphinxsearch.com/
 [7]: https://bugzilla.mozilla.org/show_bug.cgi?id=554740
 [8]: http://github.com/clouserw/tower
 [9]: https://bugzilla.mozilla.org/show_bug.cgi?id=556810
 [10]: http://github.com/jbalogh/zamboni/tree/master/apps/minify/
 [11]: http://github.com/jbalogh/zamboni/
 [12]: https://bugzilla.mozilla.org/show_bug.cgi?id=550515
 [13]: https://bugzilla.mozilla.org/show_bug.cgi?id=553131
 [14]: http://hudson-ci.org/
 [15]: https://bugzilla.mozilla.org/show_bug.cgi?id=556449
 [16]: http://github.com/jsocol/flatqs
 [17]: https://bugzilla.mozilla.org/show_bug.cgi?id=554206
 [18]: http://moxie.jamessocol.com/bugstats/sumo/1.5.4
 [19]: https://bugzilla.mozilla.org/show_bug.cgi?id=554210
 [20]: https://bugzilla.mozilla.org/show_bug.cgi?id=555249
 [21]: https://bugzilla.mozilla.org/show_bug.cgi?id=554778
 [22]: https://bugzilla.mozilla.org/show_bug.cgi?id=437891