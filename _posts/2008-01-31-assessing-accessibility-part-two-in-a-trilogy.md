---
title: Assessing Accessibility (Part Two in a Trilogy)
author: James
layout: post
permalink: /assessing-accessibility-part-two-in-a-trilogy-67/
blogger_blog:
  - urbaneexistence.blogspot.com
blogger_author:
  - James
blogger_permalink:
  - /2008/01/assessing-accessibility-part-two-in.php
bitly_url:
  - http://j0.is/16Fvnng
bitly_hash:
  - 16Fvnng
bitly_long_url:
  - http://coffeeonthekeyboard.com/assessing-accessibility-part-two-in-a-trilogy-67/
dsq_thread_id:
  - 1746518812
categories:
  - Accessibility
tags:
  - Accessibility
---
<div class="image left">
  <img src="http://jamessocol.com/blog/images/accessicon.jpg" alt="Assessing sites for accessibility is much, much easier than you think." height="50" width="50" />
</div>

The word &#8220;accessibility&#8221; is nebulous at best. We think of screen readers and how much they suck (and they do) and despair. But with a few simple criteria in hand, basic assessment for accessibility can be done in a little under 30 seconds, if you&#8217;re quick with the mouse.

### Breaking Your Site

To develop criteria for assessment (I refuse to use the word &#8220;rubric&#8221; because I think it&#8217;s unnecessary) we need to first understand what kind of accommodations people need. I&#8217;ll focus on two modifications here that will accommodate almost everyone who needs accommodation at all: text size and colors.

The default text size in most modern browsers is &#8220;16 pixels.&#8221; (I quote this because that doesn&#8217;t make text the same size in every browser. Internet Explorer tends to be larger than Firefox, and everything changes between Windows, Apple, and Linux.) But that might be too small for any number of reasons, from visual impairment to sitting too far from the monitor to displaying pages on a large (or small) screen. While most readers will never second-guess our text size, for some there may be no choice.

Since the mid-1990s, we&#8217;ve all seen the standard web colors: #FFFFFF for background, #000000 for text, #0000FF for links, etc. These defaults can be changed in the browser, or by our style sheets. A quick look at the [CSS Zen Garden][1] shows the plethora of color schemes we&#8217;ve developed. But not all readers can read all colors. From color blind readers to visually impaired readers who need high-contrast color schemes to learning-disabled readers who may have trouble focusing on the text, you must assume that some people will turn off images or backgrounds, apply user style sheets, or simply turn your style sheets off completely.

### Progressive Enhancement

What happens to your site when someone does anything like this? Do you know?

The idea is that basic content should be accessible to all browsers, regardless of capabilities or settings. If a reader has images turned off, but not style sheets—if, for example, they have severe ADHD or a slow internet connection—they should still be able to read your site. A popular bad example is the default WordPress theme, Kubrick. While it&#8217;s not unreadable to most, if you turn off images, the text has no background, leaving a low-contrast black on gray.

Go to your site and turn off images. In Firefox this is in the Options panel. What do you see?

The most common problem with changing text sizes is the way text wraps in fixed-width columns. Because of background images, or simply not knowing better, columns tend to have widths in either pixels or in percent. Go to the [New York Times][2], hold &#8220;Ctrl&#8221; and hit the &#8220;+&#8221; twice (or hold &#8220;control&#8221; and use your mouse wheel; Mac users, it&#8217;s &#8220;command&#8221;). Already the text in the left column bleeds over and starts to obscure the next column over. Now try it on this site.

Go to your own site and try this. Do your columns scale with your text, or can you make words so big they cover up the words on the right?

Style sheets present a more difficult problem for most sites, but they really shouldn&#8217;t. Is your content in a logical order? If you strip the HTML does it still make sense? Did you use tables for non-tabular content? (Bad dog!) Did you properly nest headings?

Go to your site, and turn off your style sheet. In Firefox you can go View > Page Style > No Style. Does what&#8217;s left make sense?

### Done in 30 Seconds

I use a Firefox add-on called [Web Developer][3] which adds a toolbar that makes this very fast. With it you can enable or disable images and style sheets (separately) in a matter of seconds. Combine that with &#8220;control + mouse wheel&#8221; and you can assess a site for basic accessibility in under thirty seconds.

The three tests and their three desired results:

Changing the Text Size.
:   The layout should scale with the text, so there are a (roughly) consistent number of characters per line in each column, regardless of text size.

Disabling Images.
:   Turning off images should never create low-contrast text situations or completely remove content (ie: a link that is only accessible by clicking an image).

Disabling Style Sheets.
:   The page should look like an outline of itself, and as an outline, it should be in a logical order. This is hard, and most sites will fail.

### What About Screen Readers?

Screen readers pose an interesting problem. Without them, your site, no matter how accessible it may be, is inaccessible to a small percentage of readers. But screen readers suck.

The biggest problem with screen readers is that most use the Internet Explorer DOM (a few use the Firefox or Opera DOMs, instead) to read the page, instead of reading the source and style sheet themselves. (I know of no screen reader that does this, so if you do, please [let me know][4].) So all that work the W3C put into to aural style sheets and screen reader support does absolutely no good whatsoever.

The best solution I have found is to pass the no-style-sheet test, and hope that (or encourage) any users with screen readers turn off style sheets. This causes the browser to render the page as close to the source as possible, so the DOM, and hence the screen reader, get the cleanest, most linear version. If source is well-constructed, you can provide a &#8220;text-only&#8221; solution without needing a second version.

Read the rest of the series:

  * [Universal Accessibility][5]
  * [What is Universal Accessibility? (Part One in a Trilogy)][6]
  * **Assessing Accessibility (Part Two in a Trilogy)**
  * [Building Accessible Sites (Part Three in a Trilogy)][7]

 [1]: http://www.csszengarden.com/ "Visit the CSS Zen Garden"
 [2]: http://nytimes.com "Visit the New York Times."
 [3]: https://addons.mozilla.org/en-US/firefox/addon/60 "Get the Web Developer add-on."
 [4]: /contact/ "contact me."
 [5]: http://coffeeonthekeyboard.com/universal-accessibility-64/ "Universal Accessibility"
 [6]: http://coffeeonthekeyboard.com/what-is-universal-accessibility-part-one-in-a-trilogy-66/ "What is Universal Accessibility? (Part One in a Trilogy)"
 [7]: http://coffeeonthekeyboard.com/building-accessible-sites-part-three-in-a-trilogy-69/ "Building Accessible Sites (Part Three in a Trilogy)"