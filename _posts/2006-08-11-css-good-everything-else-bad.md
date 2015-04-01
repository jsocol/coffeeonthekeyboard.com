---
title: 'CSS: Good; Everything Else: Bad'
author: James
layout: post
permalink: /css-good-everything-else-bad-33/
blogger_blog:
  - urbaneexistence.blogspot.com
blogger_author:
  - James
blogger_permalink:
  - /2006/08/css-good-everything-else-bad.php
bitly_url:
  - http://j0.is/16FvFdD
bitly_hash:
  - 16FvFdD
bitly_long_url:
  - http://coffeeonthekeyboard.com/css-good-everything-else-bad-33/
categories:
  - CSS
tags:
  - Code
  - CSS
  - How To
---
Images, flash, frames. These are the &#8220;traditional&#8221; methods for making a web site &#8220;more interesting&#8221; or &#8220;better looking.&#8221; But these hold-overs from the AOL era are the bane of bandwidth, text-only browsers, and non-visual users.

HTML, of course, was originally written to define the <span style="font-style: italic">structure</span> of a web page, never it&#8217;s design. In essence, HTML was intended to be much like XML today. But, when HTML was created, there were no style-sheets, so HTML was forced to include some design information, like <font> tags and the <tt>bgcolor</tt> attribute. Developers quickly turned to images, JavaScript, frames, and Flash to make their lives easier, but in the process made everyone else&#8217;s life harder.

Images and Flash will both suck bandwidth, so why would you use them to display text or a navigation bar? Save images and Flash for what they&#8217;re supposed to be: pictures and animations. Frames reek havoc on text-only browsers and screen-readers (hence the rarely-used <noframes> tag) and have been considered blase web-design for years.

So how can you save bandwidth, insure accessibility, and still get all the effects you want? CSS. Let&#8217;s go over a few of the quick-and-easy ways to use CSS to replace the bandwidth-heavy, inaccessible design you&#8217;re using now:

<span style="font-weight: bold">1) Frames: Fixed Sidebars</span>  
Perhaps the most common use of frames was the sidebar, a window on the left (or right) that contained anything from helpful information to your site&#8217;s navigation, appeared on every page, and didn&#8217;t scroll with the rest of the site.

If you want to do the same thing, use the CSS &#8220;position: fixed&#8221; definition. For instance:

<pre>#sidebar {
position: fixed;
top: 0;
left: 0;
width: 160px;
height: 100%;
background: #00f;
}</pre>

This will make a non-scrolling, 160-pixel wide sidebar on the left of the screen. (You could always use &#8220;right&#8221; and &#8220;bottom,&#8221; too.) Now, in your HTML file, create a <div> with the <tt>id="sidebar"</tt> attribute:

<pre>&lt;div id="sidebar"&gt;
...all your links go here...
&lt;/div&gt;</pre>

You can put this anywhere in the HTML source, and it will appear in the correct place. In a good screen-reader, if you put this toward the bottom of the HTML source (for instance, after all the content of the page) then the user would hear the content before having to hear the list of links again.

<span style="font-weight: bold">2) Images: Positioning Text</span>  
This is really just poor web-design. Unless it&#8217;s desperately important that you use a particular, rare font, you should never use images to display text. If it&#8217;s important that the text appear a certain way or in a certain position, make use of the box-model and nested <div>s. For example, let&#8217;s say you wanted a 480-pixel wide layout, regardless of the users&#8217; screen resolution, with a right gutter and a title and slogan in the top-left. You might do something like:

<pre>#main {
width: 480px;
height: 100%;
margin: 0 auto;
padding: 0;
position: relative;
top:0;
left:0;
}
#gutter {
width: 120px;
height: 100%;
margin: 0;
padding: 0;
border-left: 1px solid #000;
position: absolute;
top: 0;
right: 0;
background: #2c2;
color: #fff;
}
#header {
width: 360px;
margin: 0;
padding: 6px;
position: relative;
top: 0;
left: 0;
font-family: Tahoma, Arial, san-serif;
font-size: 140%;
text-align: center;
}</pre>

With the corresponding HTML:

<pre>&lt;div id="main"&gt;
&lt;div id="gutter"&gt;
...gutter code here...
&lt;/div&gt;
&lt;div id="header"&gt;
&lt;p&gt;Page Title&lt;/p&gt;
&lt;p&gt;Page Slogan&lt;/p&gt;
&lt;/div&gt;
&lt;/div&gt;</pre>

Which gives something like (borders added to show box model):

![][1]

Now, of course, with just a little editing of the CSS file (and none of the HTML) you can move the gutter, change the background, change the page width, change the font, and it&#8217;s all in text-format, that will display correctly to text-only or visually-impaired browsers, and save huge percentages of bandwidth.

<span style="font-weight: bold">3) Flash: Cool Navigation Bars</span>  
This gets a little more complicated. In general, it&#8217;s hard to define the box around <a> tags, so you&#8217;ll probably want to do something like this:

<pre>#nav
{   margin: 4px auto 0 auto;
 position: relative;
 top: 0;
 left: 0;
}
#nav ul
{   display: inline;
 list-style-type: none;
}
#nav li
{   display: inline;
 border: 1px solid #000;
 padding: 2px 4px 0 4px;
 margin: auto 4px 0 4px;
}
#nav li.here
{   border-bottom: 1px solid #FFF;
}
#nav li:HOVER
{   border-bottom: 1px solid #FFF;
 font-weight: bold;
}</pre>

Now in each <li> tag you&#8217;ll make a link to the page you want. This particular set of code gives you a horizontal list. I use a version of this on my [projects page][2], so you can see it in action. If you remove the <tt>display: inline</tt> parts, you&#8217;ll get the vertical list you need for a column. Of course, the margin, padding, and all the colors can be customized however you want, to make it look right. (I got this from the [AListApart][3] article &#8220;[Taming Lists][4]&#8220;, which you should read for more information.)

<span style="font-weight: bold">So there you go&#8230;</span>  
It&#8217;s not all the info you&#8217;ll ever need, but it solves three of the most heinous wastes of bandwidth I&#8217;ve seen on the web, as well as all the problems people have with accessibility. These methods can both look great, and be completely usable by everyone, something Flash, frames, and images can&#8217;t.

 [1]: http://jamessocol.com/images/screenshot.png
 [2]: http://projects.jamessocol.com "Projects page."
 [3]: http://www.alistapart.com
 [4]: http://www.alistapart.com/articles/taminglists/