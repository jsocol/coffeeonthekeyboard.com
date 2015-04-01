---
title: 'WordPress Plugin: Insert Ad Code'
author: James
layout: post
permalink: /wordpress-plugin-insert-ad-code-63/
blogger_blog:
  - urbaneexistence.blogspot.com
blogger_author:
  - James
blogger_permalink:
  - /2007/12/wordpress-plugin-insert-ad-code.php
bitly_url:
  - http://j0.is/16FvoYe
bitly_hash:
  - 16FvoYe
bitly_long_url:
  - http://coffeeonthekeyboard.com/wordpress-plugin-insert-ad-code-63/
tags:
  - Code
  - WordPress
---
I wanted a way to generate more revenue off [my WordPress blog][1]. A lot of sites use square ads embedded after the first couple of paragraphs of their articles, and I thought this would be perfect for my post pages, which don&#8217;t show the WordPress sidebar. But I couldn&#8217;t find a way to do it, so I wrote **Insert Ad Code** to do it for me.

**Insert Ad Code** is a pretty simple WP plugin: it inserts some HTML into your posts. It&#8217;s very easy to install and pretty easy to use.

**Installation**

To install **Insert Ad Code**, all you need to do is unzip or upload the `insert_ad_code` directory to your `wp-content/plugins` directory. Then activate it through the Plugins page of the site admin. That&#8217;s it, everything&#8217;s ready to go.

**Using Insert Ad Code**

Before you start using **Insert Ad Code**, you&#8217;ll need to have some kind of ad to display. I use the [Openads Ad Server][2] on my site, but you could also use [Google AdSense][3] or any other add server.

**Important Note:** **Insert Ad Code** does not rotate ads by itself. It inserts the same code every time its used. If you only have one advertiser at a time, this might be fine for you, otherwise, you&#8217;ll need to use some external program to rotate the ads.

For now, I&#8217;ll assume you&#8217;re using AdSense. Go get the AdSense code from Google. I recommend either a box ad (250&#215;250) or some kind of horizontal ad (banner or half-banner, depending on your theme), since these will get inserted directly into the post.

For example, your code might look something like this:

>     <script type="text/javascript"><!--
>     google_ad_client = "pub-xxxxxxxxxxxxxxxxx";
>     //Inserted into Articles
>     google_ad_slot = "xxxxxxxxxxxxxxx";
>     google_ad_width = 250;
>     google_ad_height = 250;
>     //--></script>
>     <script type="text/javascript"
>     src="http://pagead2.googlesyndication.com/pagead/show_ads.js">
>     </script>

Copy the code for your ad and visit your WordPress Options page. Go to the **Insert Ad Code** options. Paste the code into the text box at the bottom. Click **Update Options »**.

**Customizing Insert Ad Code**

Now you have a couple of choices here. The first option (*Enable Insert Ad Code*) will turn the plugin on or off (turning it off does not disable it in WordPress). This makes it easy to stop inserting ads if you need to.

The second (*Use <!&#8211;more&#8211;> or Custom Tag*) and third (*Custom Tag*) options let you decide where **Insert Ad Code** will put the ads. The default option is to insert the ads right by the **<!&#8211;more&#8211;>** tag. You could also use a custom tag (I&#8217;d still recommend using an HTML comment, just in case you turn off **Insert Ad Code** later).

I use the **<!&#8211;more&#8211;>** tag for four reasons.

  1. I don&#8217;t want to break up very short posts with ads.
  2. I don&#8217;t want to go back and insert a new tag into all my old posts.
  3. I want the ads to appear once, after a couple of paragraphs, just like the **<!&#8211;more&#8211;>** tag.
  4. I want the tag to be easy to add via the WordPress editor.

If you do feel like using a custom tag just change the settings, include that tag in your posts, and watch the magic happen.

**License**

This plugin is &#8220;show me&#8221;-ware. If you decide to use it, please let me know so I can see my handiwork in action! Other than that, it&#8217;s GPL.

**Download Insert Ad Code**

Now that I&#8217;ve been talking it up so much, you probably want to [download the plugin][4].

**Update!** You can now check **Insert Ad Code** from SVN to get the latest build.  
`svn://jamessocol.com/insert_ad_code/trunk`

You can check out the `tags` directory for specific versions.

**Feedback**

Please leave a comment or <a href="/contact/" onclick="return mail('me');">email me</a> if you use **Insert Ad Code**, or if you try to use it and hate it, or if it kills your dog. I&#8217;m not going to bring your dog back to life, but I&#8217;d love to hear your feedback.

**Update (Again)!** If you&#8217;d like to help me translate Insert Ad Code, version 1.1.0 and above contain `.po` files. If you&#8217;re bilingual, translate it and send me the `.po` and `.mo` files, and I&#8217;ll include them (with credit, of course!)

 [1]: http://gameindependent.com/
 [2]: http://www.openads.org/
 [3]: http://www.google.com/adsense/
 [4]: http://jamessocol.com/projects/files/insert_ad_code.zip