---
title: 'How to: Upgrade or Recompile PHP on RHEL5 (Outdated)'
author: James
layout: post
permalink: /how-to-upgrade-or-recompile-php-on-rhel5-59/
blogger_blog:
  - urbaneexistence.blogspot.com
blogger_author:
  - James
blogger_permalink:
  - /2007/10/how-to-upgrade-or-recompile-php-on.php
bitly_url:
  - http://j0.is/14dZE7s
bitly_hash:
  - 14dZE7s
bitly_long_url:
  - http://coffeeonthekeyboard.com/how-to-upgrade-or-recompile-php-on-rhel5-59/
dsq_thread_id:
  - 1746518882
categories:
  - PHP
tags:
  - How To
  - PHP
  - recompile
  - red hat
  - servers
  - software
---
**Update: ***This post is nearly two years old, and this is **not** how I would recommend upgrading PHP on RHEL, yet it continues to get traffic. If I can get my hands on a copy of RHEL, I&#8217;ll update this (or I might try using Fedora just to compare).*

Upgrading PHP on RHEL 5 is difficult. Having done it on several servers, I&#8217;ve gotten it down to a 15 to 20 step process. It takes a while, but it&#8217;s straightforward. I thought I&#8217;d share, because help was sparse and noncontiguous at best.

<p class="image right">
  <img src="/blog/images/rhel.png" alt="RedHat Enterprise Linux: Hard to upgrade PHP." width="96" height="31" />
</p>

RedHat Enterprise Linux 5 comes with PHP 5.1.6 and, as of this writing, this is the highest version available on yum. If you want to upgrade to 5.2.4, or even recompile 5.1.6 with a custom configuration, you&#8217;ll need to resolve several dependencies, first.

<p class="note">
  Unless otherwise specified, whenever I run <code>./configure</code>, I always include <code>--enable-shared</code> and <code>--enable-static</code>.
</p>

The first step is to make sure you have a working APXS script installed. None of the servers on which I&#8217;ve done this had it. I installed Apache 2.2.4 over the default install, since it was the latest version. Be sure to enable APXS with `--enable-so`. Be careful configuring Apache, as it likes to install itself in `/usr/local/apache2/` instead of `/etc/httpd/`, which you may prefer.

Now we start resolving the dependencies. I&#8217;d start with **libtool** and **libiconv**. The former you should be able to install via yum. The latter you may have to download, and after you configure it, from the source directory copy `m4/iconv.m4` to `/usr/local/share/aclocal/iconv.m4`.

Use yum to make sure `mysql-devel` is installed, you&#8217;ll need it to link to mysql.

Then I&#8217;d do the image manipulation software, since it&#8217;s fairly easy. Use yum to install `libjpeg`, `libpng` and `freetype`. You can then use yum to make sure both `gd` and `gd-devel` are installed.

I installed `libmcrypt`, `libmhash`, and `ming` at this point. I&#8217;d say it&#8217;s a good time to get any of these more particular dependencies out of the way. I also installed Tidy, which you need to check out from their CVS repository. You can run `build/gnuauto/setup.sh` from the Tidy source directory to create the autoconf files.

Now we get to the crux of the matter: configuring PHP. All the major dependencies should have been taken care of. If you have other PHP options you&#8217;ll need, make sure those prerequisites are installed, as well. Run the configure script in the PHP source directory with everything you need enabled. I find it helpful to create a script like `php.config` with the following format:

<pre>'./configure' \
'--with-cgi' \
'--with-fastcgi' \
'--with-gd' \
...
'--with-xml' \
"$@"</pre>

<p class="note">
  You need to include the slashes <code>\</code> at the end of every line. The last line, <code>"$@"</code> makes the script output the output of <code>configure</code>.
</p>

If you get an error running `make`, you may need to edit your `Makefile`. Find the `EXTRA_LIBS` section (in vi/vim, type `<ctrl-c> /EXTRA_LIBS <return>`) and add `-liconv` to the end of the line. Then try `make clean && make` and it should work.

You may or may not have to edit your `httpd.conf`, after running `make install` from the PHP source directory, to add the `AddType` or `AddHandler` directive for PHP.

That should be it. You can install extensions via PECL or Pear and everything should run. Save the source directory and your `php.config` (or `config.nice`) file, and you&#8217;ll be able to recompile at any time, in case you forgot something. (I, for example, forgot to add `--with-mysql` the first time!)

Let me know if you run into other problems. Most can be solved by typing `yum install ###-devel` to resolve a dependency, but if not, I&#8217;ve done this enough to be of some help.