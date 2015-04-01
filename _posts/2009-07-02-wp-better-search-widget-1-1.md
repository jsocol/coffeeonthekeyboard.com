---
title: 'WP: Better Search Widget 1.1'
author: James
layout: post
permalink: /wp-better-search-widget-1-1-232/
bitly_url:
  - http://j0.is/16FvoY5
bitly_hash:
  - 16FvoY5
bitly_long_url:
  - http://coffeeonthekeyboard.com/wp-better-search-widget-1-1-232/
dsq_thread_id:
  - 1761392375
categories:
  - Articles
tags:
  - Code
  - l10n
  - PHP
  - release
  - widget
  - WordPress
---
[Better Search Widget 1.1][1] is a significant upgrade to [Better Search Widget][2] that adds new features and fixes an old bug with internationalization.

### Features

(New features in bold.)

  * **Optional default value**.
  * **Optional,** custom widget title**.**
  * **Optional onfocus and onblur listeners.**
  * **Optional, customizable focus and blur colors.**
  * Custom button value.
  * Custom field size.

The built-in search widget has only one of these features, the optional, custom title.

#### Onfocus and Onblur

In order to use the blur and focus colors, you must enable the onfocus and onblur event listeners. In order to use the listeners, you must specify a default value (otherwise none of this makes sense). Here&#8217;s an example:

<div style="border: 1px solid #333; margin: 0.5em auto; padding: 0.7em 0; width: 50%; text-align: center;">
</div>

### Bug Fixes

A pretty serious typo meant that none of the internationalization code worked correctly. This has been fixed, and en\_US, en\_GB, and fr\_FR localizations are available. de\_DE is coming. If you&#8217;d like to translate, there is a .pot file included in the languages directory.

### License

Better Search Widget is released under the [MIT License][3]. If you use it, or have suggestions for new features or bug fixes, let me know!

### Getting It

You can download [Better Search Widget 1.1][1] now in a Zip file. Or, to save yourself some trouble,  you can check it out of Subversion from

<pre>svn co svn://jamessocol.com/better-search-widget/tags/1.1.0 ./better-search-widget</pre>

(Run that in your `wp-content/plugins` directory.) Subversion will make it easiest to upgrade later.

### Roadmap

Soon, though probably not today, I will be releasing Better Search Widget 2, which will take advantage of the new Widget API in WordPress 2.8. This will add support for multiple instances of the widget, but will require at least WordPress 2.8. You should upgrade, anyway.

 [1]: http://coffeeonthekeyboard.com/wp-content/uploads/2009/07/better-search-widget.zip
 [2]: http://coffeeonthekeyboard.com/wp-plugin-better-search-widget-113/ "Better Search Widget"
 [3]: http://www.opensource.org/licenses/mit-license.php