---
title: 'Firefox: Open in Blank Tab'
author: James
layout: post
permalink: /firefox-open-in-blank-tab-197/
bitly_url:
  - http://j0.is/16FvsXQ
bitly_hash:
  - 16FvsXQ
bitly_long_url:
  - http://coffeeonthekeyboard.com/firefox-open-in-blank-tab-197/
dsq_thread_id:
  - 1746519018
categories:
  - Articles
tags:
  - firefox
  - javascript
  - links
  - tricks
---
If you don&#8217;t use Firefox 3, [go get it][1]. Then finish this article. (Safari and Opera users are excused, but there&#8217;s no promise this will work for them.)

One of my (few) gripes with Firefox is that bookmarks on the toolbar have no &#8220;open in blank tab&#8221; option. They have an &#8220;open in sidebar&#8221; option, but those uses are rare and esoteric at best. Personally, I never use the sidebar.

&#8220;Open in blank tab&#8221; should basically do this: if there is a blank tab, use it; if not, create a new tab. Frankly, it could just open in a new tab regardless, but it seems like such an easy thing to add.

But? It can&#8217;t be done directly in Firefox. Hence, I present this small script:

<div class="dean_ch" style="white-space: wrap;">
  <ol>
    <li class="li1">
      <div class="de1">
        javascript:
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        <span class="br0">&#40;</span><span class="kw2">function</span><span class="br0">&#40;</span><span class="br0">&#41;</span><span class="br0">&#123;</span><span class="kw2">var</span> u=<span class="st0">&#8216;http://mail.google.com/mail&#8217;</span>;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; <span class="kw1">if</span><span class="br0">&#40;</span>window.<span class="me1">location</span>==<span class="st0">&#8216;about:blank&#8217;</span><span class="br0">&#41;</span><span class="br0">&#123;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; window.<span class="me1">location</span>=u;
      </div>
    </li>
    
    <li class="li2">
      <div class="de2">
        &nbsp; <span class="br0">&#125;</span><span class="kw1">else</span><span class="br0">&#123;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; window.<span class="kw3">open</span><span class="br0">&#40;</span>u,<span class="st0">&#8221;</span><span class="br0">&#41;</span>;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; <span class="br0">&#125;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        <span class="br0">&#41;</span><span class="br0">&#40;</span><span class="br0">&#41;</span>;
      </div>
    </li>
  </ol>
</div>

That&#8217;s it. Try dragging this link to [GMail][2] to your bookmark toolbar. Then click the link on your toolbar. Now, open a new tab, and click the link again.

This isn&#8217;t exactly what I asked for. It has no way of knowing if any blank tab exists, only if the current tab is blank. And, of course, it lacks the nice favicon support.

But it does the job. If you change the variable `u` to something other than &#8216;http://mail.google.com/mail&#8217;, you can make the link open any other page.

I love anonymous functions.

**Update:** If you want a bookmark for something besides GMail, you can [create your own][3]. Or you can drag *this* link to your toolbar, to make new ones whenever you want: [Open in Blank Tab][4].

**Update 2:** Oops, fixed the &#8220;create your own&#8221; link. Tested it, then accidentally pasted in the results, instead of the actual script.

 [1]: http://www.mozilla.com/en-US/
 [2]: javascript:(function(){var%20u='http://mail.google.com/mail';%20if(window.location=='about:blank'){window.location=u;}else{window.open(u,'');}})();
 [3]: javascript:(function(){var u=prompt('Enter the URL to open in a blank tab.','http://');prompt('Copy the text below into a new bookmark.',"javascript:(function(){var u='"+u+"';if(window.location=='about:blank'){window.location=u;}else{window.open(u,'');}})();");})();
 [4]: javascript:(function(){var u=prompt('Enter the URL to open in a blank tab.',window.location);prompt('Copy the text below into a new bookmark.',"javascript:(function(){var u='"+u+"';if(window.location=='about:blank'){window.location=u;}else{window.open(u,'');}})();");})();