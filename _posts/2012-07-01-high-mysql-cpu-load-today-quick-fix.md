---
title: High MySQL CPU Load Today? Quick Fix
author: James
layout: post
permalink: /high-mysql-cpu-load-today-quick-fix-684/
bitly_url:
  - http://j0.is/14gBJ7z
bitly_hash:
  - 14gBJ7z
bitly_long_url:
  - http://coffeeonthekeyboard.com/high-mysql-cpu-load-today-quick-fix-684/
dsq_thread_id:
  - 1746519623
categories:
  - Articles
---
If you started seeing a load spike in MySQLd (or apparently Java) processes this morning, it may be the fault of yesterday&#8217;s leap second.

Apparently due to [tides slowing the rotation of the earth][1], there was an extra second added to 30 June 2012, so at midnight GMT (8pm Eastern, 5pm Pacific, in the US), something funny happened:

  * 23:59:59 30/06/2012
  * 23:59:60 30/06/2012
  * 01:01:00 01/07/2012

*What?* It can do that? [Oh yeah][2], and it can do [so much worse][3]. This was nothing. Don&#8217;t get me started [on timezones][4].

And yet MySQLd and Java apparently didn&#8217;t like it, at least some version and OS combinations. Fortunately, there&#8217;s an easy fix. [Sheeri][5] from Mozilla&#8217;s IT group pointed me to [their blog post][6].

But if you aren&#8217;t dealing with [puppet][7] or anything, this is all you need to do:

    # Log in as root
    $ /etc/init.d/ntpd stop
    $ date -s "`date`"
    $ /etc/init.d/ntpd start
    

I didn&#8217;t even restart MySQLd and its CPU usage plummeted back to normal.

Now if that&#8217;s not the solution, you&#8217;ll have to start looking at things like [`SHOW PROCESSLIST`][8] and the [slow query log][9] and [`EXPLAIN`][10] to figure out what&#8217;s wrong. Or maybe you just have [too much traffic][11]. (Yay!)

 [1]: https://twitter.com/neiltyson/status/219042429653889026
 [2]: http://infiniteundo.com/post/25326999628/falsehoods-programmers-believe-about-time
 [3]: http://infiniteundo.com/post/25509354022/more-falsehoods-programmers-believe-about-time-wisdom
 [4]: http://butgrace.com/2012/06/19/more-falsehoods-programmers-believe-about-time/
 [5]: https://twitter.com/sheeri/status/219399191942799360
 [6]: http://blog.mozilla.org/it/2012/06/30/mysql-and-the-leap-second-high-cpu-and-the-fix/
 [7]: http://puppetlabs.com/
 [8]: http://dev.mysql.com/doc/refman/5.5/en/show-processlist.html
 [9]: http://dev.mysql.com/doc/refman/5.5/en/slow-query-log.html
 [10]: http://dev.mysql.com/doc/refman/5.5/en/explain.html
 [11]: http://coffeeonthekeyboard.com/why-django-sucks-except-when-it-doesnt-664/