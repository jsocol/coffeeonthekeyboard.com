---
title: 'Testing with Django&#8217;s Cache'
author: James
layout: post
permalink: /testing-with-djangos-cache-1229/
pw_single_layout:
  - 1
dsq_thread_id:
  - 3297476581
categories:
  - Articles
tags:
  - django
  - python
  - testing
---
I don&#8217;t love my solution to this problem, so I&#8217;m writing about it in hopes that someone has something better.

When you run tests with Django, you get an [isolated test database](). This can be wiped out and the consistency makes life a lot easier when you are running tests locally in a dev environment.

However, [caches are not cleared][1] or isolated. Since the database *can be* cleared, that means that any cache keys based on model primary keys are likely to recur.

That sucks.

Here are some of the compounding challenges:

  * The application code needs to reach into the internals of the cache backend, for various reasons, so just using the `LocMem` backend doesn&#8217;t work. I don&#8217;t want to maintain a second custom cache backend.
  * My dev environment, where I&#8217;m also running tests, is a VM that is very close to production, on purpose. I&#8217;d rather not run a second `memcached` process if possible, at least not all the time.
  * The main `memcached` process also support the local running services that I use for manual and integration testing. It would be acceptable to flush this before or after tests but not ideal.

I don&#8217;t want to maintain a new test runner but I&#8217;m happy to wrap the test run commands in a shell script (I already use an alias for my common options). I&#8217;m pretty much deciding between:

  * Just flush the whole cache before and after running the tests. It&#8217;s a blunt but effective hammer. It does not require an extra settings module. But, anything I do while the tests are running changes the environment.
  * Add a test settings module with a random (at import time) `KEY_PREFIX`. This seems effective so far. It isolates each test run, and anything I do while the tests are running. It does require a new settings module. It leads to garbage in the cache, but the VM is usually OK on memory headroom. I can always manually flush it, too.

Of these, I&#8217;ll probably do the latter. Real isolation from running processes seems worth the other maintenance overhead.

What do you do? It seems like nearly all roads lead to separate test settings, but which road do you take?

 [1]: https://docs.djangoproject.com/en/1.6/topics/testing/overview/#other-test-conditions