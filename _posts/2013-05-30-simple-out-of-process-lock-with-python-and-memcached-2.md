---
title: Simple out-of-process lock with Python and Memcached
author: James
layout: post
permalink: /simple-out-of-process-lock-with-python-and-memcached-2-985/
pw_single_layout:
  - 1
bitly_url:
  - http://j0.is/14dZPQi
bitly_hash:
  - 14dZPQi
bitly_long_url:
  - http://coffeeonthekeyboard.com/simple-out-of-process-lock-with-python-and-memcached-2-985/
dsq_thread_id:
  - 1746519774
categories:
  - Articles
tags:
  - cas
  - Code
  - lock
  - memcached
  - python
  - scale
---
On [TodaysMeet][1] I need to check that a name is not in use before creating a new record. Unfortunately, because names can be reused over time, I can&#8217;t create a `UNIQUE` key in the database and enforce it there. That means there is some tiny amount of time between checking for existence and writing a new record.

Normally, the gap is way too small to matter. But every once in a while some client will send multiple `POST` requests at more or less the same time. (The internet is a [weird place][2] at scale.)

Since the rest of the application relies on having only one &#8220;active&#8221; name at a time, writing the row twice causes problems throughout the app*.

The concept is simple. I want to acquire a lock on the name so that only one thread/process/server at a time can actually do a check and write. On a single process, or even across processes on a single server, Python has tools for this [built in][3]. (Assuming your concurrency model works with `multiprocessing`, I guess.)

But I&#8217;d like to be able to run this across multiple machines and scale horizontally. Fortunately, I have [memcached][4] and memcached has an atomic `add` operation.

`add` works perfectly for this when multiple processes might be writing to the cache. Unlike `set`, `add` will only succeed *if the key does not exist*. Boom.

Writing this up as a little context manager (the `cache` object here stands in for any memcached client, in my case the Django wrapper around it):

    import hashlib
    from contextlib import contextmanager
    
    @<a href="http://twitter.com/contextmanager">contextmanager</a>
    def name_lock(name):
        key = 'namelock::%s' % hashlib.md5(name.encode('utf-8')).hexdigest()
        lock = cache.add(key, True, expires=10)  # In case the power goes out.
        yield lock  # Tell the inner block if it acquired the lock.
        if lock:  # Only clear the lock if we had it.
            cache.delete(key)
    

Now I can have multiple processes on multiple machines safely check the name existence and create a row.

    with name_lock(name) as lock:
        if lock and name_is_valid(name):
            save_name(name)
    

Is that the best way to do it? Probably not. If you can improve on this, or have an entirely better idea, I&#8217;d love to hear it!

*: The other interesting column is a datetime: `expires`. I&#8217;ve been bitten too often before by seconds ticking over to bother with a unique key on `(name, expires)`.

 [1]: https://todaysmeet.com/
 [2]: http://coffeeonthekeyboard.com/our-daily-errors-941/
 [3]: http://docs.python.org/2/library/multiprocessing.html#synchronization-between-processes
 [4]: http://memcached.org/