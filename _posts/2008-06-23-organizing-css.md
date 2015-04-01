---
title: Organizing CSS
author: James
layout: post
permalink: /organizing-css-105/
bitly_url:
  - http://j0.is/16Fvori
bitly_hash:
  - 16Fvori
bitly_long_url:
  - http://coffeeonthekeyboard.com/organizing-css-105/
categories:
  - CSS
  - Design
  - Standards
tags:
  - Code
  - CSS
  - Design
  - Front-end
  - organization
  - readability
  - Standards
---
Looking at WordPress themes usually makes me cringe. It&#8217;s as if there was a memo on semantic markup and the community of WP developers didn&#8217;t get it.

[Some themes][1] waste kilobytes of HTML source on something that could be achieved with 75% less markup. Some use blatantly non-compliant code. Almost none use [semantic names][2].

But what really irks me—I&#8217;ll cop to using meaningless code to make it look good—is the style of CSS that seems to be spreading: breaking up definitions into a half-dozen chunks, no line breaks, lack of organization. I think their heart is in the right place (a section for colors, so don&#8217;t have to worry about layout; a section for typography, so the precious padding is protected) but the result is a horrid mess.

I [blame Michael Heilemann][3], the designer behind the bland and semantic-free default WordPress theme. I imagine theme developers, many just starting with HTML and CSS, started by looking at his code, and thought that was the way to do it. Then it spread like a virus.

Here&#8217;s an example from &#8220;Autumn Concept 1.0&#8243;:

<pre>#topbar {background-color: #4b7c44;}
#footer {background-color: #4b7c44;}
#mainpicinner {height: 250px; background: «
  url(images/mainpic01.jpg) top left «
  no-repeat #fff; border: 1px solid #fff;}
/* typography */
#logo a {color: #3a4032;}
.textbkg {border-left: 4px solid #ebf0cf;}</pre>

<p class="note">
  (« is an inserted linebreak.)
</p>

Wow. Line breaks? Readability? Was this passed through a bad version of [JSMin][4]?

This is from the &#8220;Color Scheme&#8221; section, but the first directive for `#mainpicinner` is `height`. It also has a `border`, not just `border-color` but the whole thing. What&#8217;s the point of having sections if you proceed to ignore them immediately?

The rest is filled with classes like `cols01` and `box01` (while there are other `cols##`, there is no `box02`).

But that isn&#8217;t my real problem. My real problem is about 20 lines further down:

<pre>body {position: relative; background: #1f1f1f; «
  font: 70% Verdana, Arial, Helvetica, «
  sans-serif; text-align: center; letter-spacing: 0;}
#container {float: left; display: block; width: 100%;}
#topbar {float: left; display: block; width: «
  100%; background-image: url(images/topbar.png); «
  background-position: top; background-repeat: «
  repeat-x; text-align: left; padding: 13px 0 6px 0;}
#topbar div {padding-bottom: 0;}</pre>

#container is back. (As are backgrounds. Pick a spot, already!)

This kind of CSS is hard to read, hard to maintain, and hard to customize. Even if the initial version is perfect—which doesn&#8217;t exist—things will start to break as soon as someone opens the file. Even in this published style sheet, the *author* couldn&#8217;t decide if background images and borders belonged in &#8220;Color Scheme&#8221; or &#8220;General Styles.&#8221; What chance does a maintainer have?

I am, admittedly, obsessively strict with [my style sheets][5]. I like to make very sure that every style affects only what I intend it to affect. But I never let the styles for one element single get broken into two places. Instead, what I try to do is keep similar styles in a similar order inside those blocks:

<pre>blockquote.dropquote {
  float: right;  font-family: Arial, «
    Helvetica, sans-serif;
  font-size: 130%;

color: #662020;
  background-color: #ddd;
}

div.login {
  position: absolute;
  top: 0;
  left: 0;

  font-size: 80%;

  color: #fff;
}</pre>

Get the idea? Within each selector, I try to keep things in the same order. I almost always keep positioning styles first and then do either typography or color. To me, this is much more readable and maintainable. If my header div is 3 pixels too wide, I don&#8217;t have to comb through all the `#header` sections. I go to *one* place and fix it.

I like to [extract the CSS order from the document order][6]. This doesn&#8217;t necessarily stay complete or strict, especially when you have classes that can be used anywhere or you&#8217;re controlling tags directly. The header styles do come *before* the content styles, though, which come *before* the footer styles. That just makes *sense*.

Am I the only one who can&#8217;t stand this &#8220;style&#8221; of CSS? Do you use it? Why?

 [1]: http://themes.wordpress.net/columns/2-columns/2991/autumn-concept-10/
 [2]: http://coffeeonthekeyboard.com/use-semantics-to-guide-design-53/ "semantic names"
 [3]: http://coffeeonthekeyboard.com/the-new-blog-89/ "blame Michael Heilemann"
 [4]: http://www.crockford.com/javascript/jsmin.html
 [5]: http://coffeeonthekeyboard.com/wp-content/themes/default/style.css
 [6]: http://coffeeonthekeyboard.com/work-pattern-designing-web-sites-93/ "extract the CSS order from the document order"