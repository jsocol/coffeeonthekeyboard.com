---
title: '(Don&#8217;t) Close Firefox with Last Tab'
author: James
layout: post
permalink: /dont-close-firefox-with-last-tab-296/
bitly_url:
  - http://j0.is/16Fvuiy
bitly_hash:
  - 16Fvuiy
bitly_long_url:
  - http://coffeeonthekeyboard.com/dont-close-firefox-with-last-tab-296/
dsq_thread_id:
  - 1746519357
categories:
  - Articles
tags:
  - about:config
  - config
  - firefox
  - tip
---
Prior to Firefox 3.5, the default behavior of closing the last tab was to leave a blank tab. That changed in Firefox 3.5, and the default behavior is now to close the whole window with the last tab. If you&#8217;re on Windows or tend to close tabs with Ctrl-W, like me, this can be pretty annoying if you forget about it, what with reopening the browser and all.

Fortunately, it&#8217;s easy to change.

Venture into about:config. Go to the address bar and enter &#8220;about:config&#8221; and press enter. You&#8217;ll see a long list and at the top, a text box called &#8220;Filter:&#8221;

<img class="aligncenter size-full wp-image-297" title="closewindowwithlasttab" src="http://coffeeonthekeyboard.com/wp-content/uploads/2009/09/closewindowwithlasttab.png" alt="closewindowwithlasttab" width="250" height="182" />**Buyer Beware here**. You probably saw a warning when you tried to go to about:config. That&#8217;s because you can significantly alter the behavior of Firefox here, and you need to be either very careful about what you change, or very confident in your ability to fix it.

Enter &#8220;tabs&#8221; into the filter and you&#8217;ll see a list like the picture above. There may be more items depending on the [addons][1] you have installed. Now look for &#8220;browser.tabs.closeWindowWithLastTab&#8221; and double click it. It should turn bold, and in the right columns it will say &#8220;user set&#8221; &#8220;boolean&#8221; &#8220;false&#8221;.

That&#8217;s it!

Of course, if you&#8217;re not comfortable mucking about in about:config, or you also want to restore the *close* button to that last tab, there&#8217;s an [addon][2]* for that.

* Of course, I haven&#8217;t used, and can&#8217;t vouch for, that addon, but it&#8217;s there.

<div id="_mcePaste" style="overflow: hidden; position: absolute; left: -10000px; top: 125px; width: 1px; height: 1px;">
  <img src="file:///C:/Users/James/AppData/Local/Temp/moz-screenshot-2.png" alt="" />
</div>

 [1]: http://addons.mozilla.com/
 [2]: https://addons.mozilla.org/en-US/firefox/addon/12991