---
title: 'WP Plugin: Better Search Widget'
author: James
layout: post
permalink: /wp-plugin-better-search-widget-113/
bitly_url:
  - http://j0.is/14dZxsE
bitly_hash:
  - 14dZxsE
bitly_long_url:
  - http://coffeeonthekeyboard.com/wp-plugin-better-search-widget-113/
dsq_thread_id:
  - 1749333008
categories:
  - Design
tags:
  - Back-end
  - Code
  - Design
  - Projects
  - search
  - widget
  - WordPress
---
Today I upgraded from WordPress 2.3.3 to 2.6.1. I&#8217;m such a late adopter sometimes.

I had to go through and repeat a few hacks. For example, 2.3.x didn&#8217;t allow you to do `get_sidebar($name)`, so I&#8217;d hacked the &#8220;get_sidebar()&#8221; function. And I replaced the still-broken Atom feed reading widget with James Wilson&#8217;s [Google Reader Widget][1].

Then I finally got fed up with the default &#8220;Search&#8221; widget, which doesn&#8217;t look like the other widgets at all (no title), so I started hacking into that one. Then I realized &#8220;why hack, when I can extend?&#8221;

So, here it is, [Better Search Widget][2].

All it does is add a search widget with a customizable title, submit button, and field size. Quick-and-useful. You can see the results in the sidebar.

If you decide to use it, leave a comment and I&#8217;ll check out your blog.

 [1]: wordpress.org/extend/plugins/google-reader-widget/
 [2]: http://jamessocol.com/projects/better-search-widget.php