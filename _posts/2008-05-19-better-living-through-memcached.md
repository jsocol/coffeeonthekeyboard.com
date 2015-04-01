---
title: Better Living through Memcached
author: James
layout: post
permalink: /better-living-through-memcached-85/
bitly_url:
  - http://j0.is/14dZQUc
bitly_hash:
  - 14dZQUc
bitly_long_url:
  - http://coffeeonthekeyboard.com/better-living-through-memcached-85/
dsq_thread_id:
  - 1746519135
tags:
  - Code
  - How To
  - instructions
  - Projects
  - quality
  - server
  - service
  - software
  - useful
  - Web 2.0
---
I wanted to put something specific in the title, like &#8220;Speed up your service&#8221; or &#8220;Reduce server load&#8221; or &#8220;Limit database calls&#8221; or&#8230; You see why I chose &#8220;Better Living.&#8221;

[Memcached][1] is a memory caching system with an obvious name. It allows you to store basically any data that can be serialized into a giant, memory-resident hash, then retrieve it with its unique key.

Imagine not querying your database on every request, and you only begin to get a sense of how useful this is.

Let&#8217;s go through a simple, single-server setup.<!--more-->

### Installing Memcached

First you&#8217;ll need to make sure [libevent][2] is installed. Easy enough through yum or apt-get.

[Download Memcached][3] and untar it. Run the `configure` script. You can probably do this with no arguments. I used `--prefix=/usr` to change from the default of`/usr/local`. On your system you may want to add `--enable-64bit` or `--enable-threads`, depending on your configuration and expected load.

Run a quick `make && make install` and you&#8217;re done.

### Running Memcached

You can run Memcached from the command line but sticking the `-d` parameter to run as a daemon. If you&#8217;re still root, you&#8217;ll need to add `-u<em>nobody</em>`, where &#8220;nobody&#8221; is some unprivileged user. And that&#8217;s it. Memcached is up and running on your server and ready to connect.

If you&#8217;re on a system with chkconfig, I&#8217;ve put together a [simple chkconfig script][4] for it. The zip file contains two files, `etc/rc.d/init.d/memcached` and `etc/sysconfig/memcached`. Copy them to those locations, `chmod +x /etc/rc.d/init.d/memcached` and then run `chkconfig --add memcached`, and `chkconfig memcached on` to run Memcached when the server starts. You can type `service memcached start` to get it up and running.

Typing `memcached -h` will show you all the options, including which ports and IP addresses to listen on, or which socket to use, how much memory to allocate, and others. You can add any of these command line options to the `$OPTIONS` variable in the `/etc/sysconfig/memcached` file to set the default options for the Memcached service.

### Using Memcached (Finally!)

Memcached is installed and running. Now what? Well it sort of depends on your language—there are Memcached bindings for Perl, PHP, Ruby, Java, Python, C, C#, Lua, and even Postgres and MySQL—and your program. I&#8217;m not going to go through the specific implementation, but here&#8217;s the basic idea. (In PHP. Ok, so I lied about that specific-implementation thing.)

Let&#8217;s say our goal is to reduce database load. Then we want to try to preempt as many SELECT queries as we can.

If your original code was like this: (Please! Be safe about SQL-injection.)

> <pre>// What post do we want?
$id = get_current_id();

// Query the database
$query = "SELECT * FROM posts WHERE id = '$id';"
$result = $db-&gt;query($query);

// Now we have the post data
$post = $result-&gt;fetch_assoc();</pre>

What we&#8217;re doing is getting an ID, possibly from a user, then getting the associated table row and returning it. Now to speed that up with Memcached:

> <pre>// Create a new Memcache object
$m = new Memcache;
$m-&gt;<a href="http://s.hort.cc/D">connect</a>('localhost');

// Check Memcache for the data, first
if ( !$post = $m-&gt;get("post:$id") ) {
  // Not in the cache, get from the database
  $query = "SELECT * FROM posts WHERE id = '$id';";
  $result = $db-&gt;query($query);

  // Get the post data
  $post = $result-&gt;fetch_assoc();

  // If we expect this data to be used again soon,
  // we can store it in the cache
  $m-&gt;set("post:$id", $post);
}</pre>

There we have all the basics. The string `"post:$id"` is the Memcached key. The post data is serialized and stored (with [Memcache::set][5]) as a key-value pair. If it&#8217;s in the cache, which we can check with [Memcache::get][6] (unless the stored value was boolean `false`, then it gets confusing) then we don&#8217;t need to waste a database query.

If you expect data to be used as soon as it&#8217;s created, you can do your database `INSERT` statements and then store the same data in the cache immediately. (Or [Memcache::replace][7], whichever is necessary.)

### So, what?

To be fair, my Memcached example made the code much longer, and required instantiating another object, another TCP connection&#8230; What&#8217;s the payoff?

Memcached was written to improve performance over at LiveJournal when they hit the 20+ million page view/day mark. Imagine that database call was in a loop, or if, instead of serializing one post, we built an array of all the posts from one user, and stored that. You can quickly see how many database queries we eliminate.

For fun, I ran a test that generated a random number, then tried to pull the associated row out of a database. (I only generated numbers for which I knew there was a row.) I iterated over this 3,000 times and did 10 trials each.

In PHP with a MySQL database, the Memcached version was around 60% faster, averaging 0.315 seconds vs. 0.820 seconds without Memcached.

### But Wait! There&#8217;s More

Since Memcached was designed for a site that already existed on dozens of servers, it&#8217;s based on the idea that you&#8217;d run Memcached on all your web servers, and then treat them like a pool. You can connect to several servers simultaneously to spread the Memcaching around. Data gets hashed to a particular server and, if it&#8217;s available on any of them, gets pulled back.

Memcached can also compress data on the fly with zlib, so if you did plan on storing the last 20 posts of every user, you wouldn&#8217;t be out quite so much space.

Finally, Memcached will keep adding data until it hits its memory limit, and which point it drops the Least Recently Used stuff to make room. So even though you can set data not to expire, you can&#8217;t trust that it will be in the cache forever.

For sites that rely on database reads, Memcached can help a lot. If you&#8217;re site depends more on writes or updates, it will probably do very little, or even cause unnecessary overhead.

But since most data-driven sites are significantly bigger readers than writers, Memcached is a great way to improve response time and quality of service, and to reduce load on those databases for when you really do need to read or write to it.

### Do You Memcached?

So, do you use Memcached? How? Does it speed up your service or have no noticeable effect?

 [1]: http://danga.com/memcached/
 [2]: http://www.monkey.org/~provos/libevent/
 [3]: http://danga.com/memcached/download.bml
 [4]: http://coffeeonthekeyboard.com/wp-content/uploads/2008/05/memcache-chkconfig.zip "Memcached Chkconfig Scripts"
 [5]: http://s.hort.cc/E
 [6]: http://s.hort.cc/F
 [7]: http://s.hort.cc/G