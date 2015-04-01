---
title: 'Mistakes: Importing Data with MySQL'
author: James
excerpt: "I blew most of the day trying to load data into MySQL because I didn't read. Hopefully my trials with 'max_allowed_packet' will help you avoid the same fate."
layout: post
permalink: /mistakes-importing-data-with-mysql-309/
bitly_url:
  - http://j0.is/1VnGKR
bitly_hash:
  - 1VnGKR
bitly_long_url:
  - http://coffeeonthekeyboard.com/mistakes-importing-data-with-mysql-309/
dsq_thread_id:
  - 1746519856
categories:
  - Articles
tags:
  - cli
  - command line
  - learn from me
  - mistake
  - MySQL
  - pipe viewer
  - pv
---
I spent the better part of today trying to import—in various attempts—between 800 MB and 3.1 GB of data into a local MySQL server. The whole time I was Doing It Wrong™. Now I feel like [this][1]:

<p style="text-align: center;">
  <img class="aligncenter size-full wp-image-310" title="Image by Zach Klein" src="http://coffeeonthekeyboard.com/wp-content/uploads/2009/11/frustrated.jpg" alt="frustrated" width="240" height="160" />
</p>

I was using [pipe viewer][2], which obscured the error for most of the day. (If there&#8217;s a way to stop `pv` from eating messages to stderr, please let me know!) But eventually I figured out it was hitting the `max_allowed_packet` limit and my import was dying. I was using the MySQL docs as my reference point, and [they imply][3] that the following should work:

<pre>$ mysql --max_allowed_packet=32M db &lt; data.sql</pre>

I know now that this does not work. And I feel like a moron. If you read carefully, which I did not, you&#8217;ll see that they&#8217;re talking about server startup, not client startup, even though the example uses &#8220;mysql&#8221; instead of &#8220;mysqld&#8221;.

When I stopped using pipe viewer, I got this series of errors:

<pre>$ mysql --max_allowed_packet=16M db &lt; data.sql
ERROR 1153 (08501) at line 175458: Got a packet \
    bigger than 'max_allowed_packet' bytes
$ mysql --max_allowed_packet=128M db &lt; data.sql
ERROR 1153 (08501) at line 175458: Got a packet \
    bigger than 'max_allowed_packet' bytes
$ mysql --max_allowed_packet=256M db &lt; data.sql
ERROR 1153 (08501) at line 175458: Got a packet \
    bigger than 'max_allowed_packet' bytes
$ mysql --max_allowed_packet=1G db &lt; data.sql
ERROR 1153 (08501) at line 175458: Got a packet \
    bigger than 'max_allowed_packet' bytes</pre>

Now here&#8217;s the thing, data.sql was just shy of 80 MB. So obviously something was wrong.

A quick edit to `/etc/my.cnf`:

<pre>[mysqld]
...
max_allowed_packet=32M
...</pre>

Then all I had to do was restart mysqld, and the next time I tried:

<pre>mysql db &lt; data.sql</pre>

It finished without a problem.

7 hours and a massive facepalm later&#8230; I hope this keeps someone from wasting the same amount of time I did on such a dumb mistake.

 [1]: http://www.flickr.com/photos/zachklein/54389823/
 [2]: http://spindrop.us/2009/09/16/getting-started-with-pipe-viewer/
 [3]: http://dev.mysql.com/doc/refman/5.0/en/using-system-variables.html