---
title: Django Fixtures with Circular Foreign Keys
author: James
layout: post
permalink: /django-fixtures-with-circular-foreign-keys-480/
bitly_url:
  - http://j0.is/14dZFIK
bitly_hash:
  - 14dZFIK
bitly_long_url:
  - http://coffeeonthekeyboard.com/django-fixtures-with-circular-foreign-keys-480/
dsq_thread_id:
  - 1746518986
categories:
  - Articles
tags:
  - Database
  - django
  - mozilla
  - python
  - testing
  - web dev
---
If you create a nice, perfectly normalized database, you (probably) won&#8217;t ever run into circular foreign keys (when a row in table A references a row in table B that references the same row in table A).

In the real world, this happens pretty regularly. The most common situation is a &#8220;current&#8221; or &#8220;last&#8221; denormalization. You don&#8217;t really want to do a subquery with a sort every time you want to know the latest post in a forum thread, or current revision of a wiki page.

The problem—one we&#8217;ve been dealing with since [we decided to rebuild SUMO][1]—is that trying to load data with circular foreign keys produces a &#8220;chicken and the egg&#8221; situation: since each row depends on the other, neither can be loaded first.

(This is part of a bigger problem with MySQL, which is that it lacks deferred foreign key checks.)

The solution to this is to temporarily disable foreign key checks while you load in data. It&#8217;s not hard, but Django is so far [unwilling][2] to do it.

Well, now we get the chance to see if their concerns are realistic: with [the latest commit][3] to [Jeff Balogh&#8217;s][4] [test-utils][5] package for Django, we&#8217;re disabling foreign key checks during fixture loading.

Both [SUMO][6] and [AMO][7] have had to do some acrobatic hackery to get around the limit. This solution is definitely a filthy hack, but it&#8217;s contained in a single, small place, rather than spread throughout test cases in multiple projects.

Suggestions for improving this hideous monkey patch are welcome, but in the meantime I&#8217;ll be removing the gross parts from [Kitsune][8] that we needed to work around this.

 [1]: http://coffeeonthekeyboard.com/the-evolution-of-sumo-339/
 [2]: http://code.djangoproject.com/ticket/3615
 [3]: http://github.com/jbalogh/test-utils/commit/ce0e9643ea3b38373823e04d8c2e5f2dc2de5665
 [4]: http://jeffbalogh.org/
 [5]: http://github.com/jbalogh/test-utils
 [6]: http://support.mozilla.com/
 [7]: https://addons.mozilla.org/
 [8]: http://github.com/jsocol/kitsune