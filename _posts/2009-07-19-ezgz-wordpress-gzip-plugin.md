---
title: 'EzGz: WordPress GZip Plugin'
author: James
layout: post
permalink: /ezgz-wordpress-gzip-plugin-268/
bitly_url:
  - http://j0.is/14dZQ6V
bitly_hash:
  - 14dZQ6V
bitly_long_url:
  - http://coffeeonthekeyboard.com/ezgz-wordpress-gzip-plugin-268/
categories:
  - Articles
tags:
  - download
  - plugin
  - WordPress
---
By default, WordPress (at least in the current generation, 2.8) does not enable any kind of compression on its output. Some plugins, like [WP Super Cache][1], add gzip compression, but that&#8217;s an awfully big plugin if all you want is compressed output.

So I wrote [EzGz][2] this afternoon. It is, by far, the simplest plugin I&#8217;ve ever written. The actual source is probably two orders of magnitude smaller than the comments (plugin details and [the license][3]).

All EzGz does is enable the [built-in PHP gzip/deflate handler][4]. This does require output buffering, so it&#8217;s not right for everyone. Some themes or other plugins may already do this.

I&#8217;ve tried using the <var>zlib.output_compression</var> ini setting, but on my server, with zlib working otherwise correctly, the setting had no effect. So I stuck with the output buffering.

You can [download it][5], or check it out of [svn][6], then turn it on and [see if it works][7].

 [1]: http://wordpress.org/extend/plugins/wp-super-cache/
 [2]: http://jamessocol.com/projects/ezgz.php
 [3]: http://www.opensource.org/licenses/mit-license.php
 [4]: http://www.php.net/ob_gzhandler
 [5]: http://jamessocol.com/projects/files/ezgz.zip
 [6]: svn://jamessocol.com/ezgz/trunk
 [7]: http://www.whatsmyip.org/http_compression/