---
title: Building Accessible Sites (Part Three in a Trilogy)
author: James
layout: post
permalink: /building-accessible-sites-part-three-in-a-trilogy-69/
blogger_blog:
  - urbaneexistence.blogspot.com
blogger_author:
  - James
blogger_permalink:
  - /2008/02/building-accessible-sites-part-three-in.php
bitly_url:
  - http://j0.is/16Fvnne
bitly_hash:
  - 16Fvnne
bitly_long_url:
  - http://coffeeonthekeyboard.com/building-accessible-sites-part-three-in-a-trilogy-69/
categories:
  - Accessibility
tags:
  - Accessibility
---
<div class="image right">
  <img src="http://jamessocol.com/blog/images/accessicon.jpg" alt="With a few key concepts in mind, building accessible websites is no different than building websites." height="50" width="50" />
</div>

So your site didn&#8217;t pass [all of the tests][1]. How do you fix it? Or better, how do you build a new site that will pass the tests and meet accessibility requirements, like WCAG and Section 508?

Start with these checklists, geared towards passing the tests in [part two][2]. Then end with one of the free, online accessibility validators (about which I&#8217;ll write soon).

### Starting Points

These should really be true for *any* web site, but are particularly important for sites that need to be accessible.

Is my (X)HTML valid?
:   Maintaining [valid code][3] is the first step to making your pages look right.

Is my CSS valid?
:   Similarly, make sure your [CSS validates][4] to ensure everything looks how you want it.

Do I maintain separation of presentation and content?
:   Keep *all* style information in external style sheets. **Do not use deprecated tags**, like `<b>` and `<i>`, instead use semantic tags like `<em>` and `<strong>`. Avoid `style` attributes, instead using `id` or `class` and keeping the styles where they belong: in a style sheet.

Do I avoid layout tables?
:   Tables are for *tabular content*. Use them for presenting data, never for layout. It&#8217;s wrong, it&#8217;s not easier, it creates huge code overhead, and layout tables are almost impossible to make accessible. Use `<div>`s and `float` directives, instead.

Do I use meaningful link text?
:   Never use *click here* as link text. Obviously we&#8217;re going to click: it&#8217;s a link, that&#8217;s just what you do. Consider these two examples:</p> 
    
      * *Bad:* To read our privacy policy, [click here][5].
      * *Good:* If you&#8217;re curious, you may want to read our [privacy policy][6].
      * *Better:* If you&#8217;re curious, you may want to [read our privacy policy][7].

### Changing the Text Size.

To ensure your layout scales with the text, try to answer &#8220;yes&#8221; to these questions:

Are all my text sizes in percents?
:   By making sure all `font-size` settings are in percent (ie: `font-size: 120%`). This makes the relative text sizes the same, regardless of base size. (Using `em`s is also acceptable, but harder to follow.)

Are all widths and heights in `em`s?
:   This applies to block-level elements containing text, like `<div>` or `<p>`*. Heights may also be in percent. Remember that the average width of one letter is `0.5em`, and that 45 to 80 characters per line is considered readable, and 66 is considered standard. That means main text content should be between `22.5em` and `40em` wide.

<p class="note">
  *: Images tags have their own height and width attributes, which must be in pixels. This is an acceptable exception. As you change the font-size, the text may not flow perfectly around images which remain fixed. Accept it.
</p>

### Disabling Images.

Disabling images should never remove content or create low-contrast situations.

Do all sections with a `background-image` also have a `background-color`?
:   If you have a background image behind your text, make sure the container also has a background color that is the same as the main color of the image. For example, if you have a gray page background with black text, and your `content` div has a white background image, it should also have a white background color.

Is all my content available as text?
:   Make sure you provide meaningful `alt` tags to your images and embeds. Some of the best advice I ever got was &#8220;if the picture isn&#8217;t worth describing well, why is it there at all?&#8221; (`alt` tags are required, anyway. The important part here is being meaningful.)
:   If you absolutely *must* replace text with an image—for example in a logo—implement a CSS [image replacement technique][8]. **Do not use [Fahrner Image Replacement][9]**, or FIR, which is incompatible with most screen readers.

### Disabling Style Sheets.

This is the hard part. While it&#8217;s relatively easy to properly structure new pages, it can be very difficult to go back and restructure old one, especially if you were bad and used layout tables.

When creating a new site, start with the content. When I start a new design, I always* lay out the content in outline order before I even begin to consider the layout.

<p class="note">
  *: Ok, that&#8217;s a lie. People don&#8217;t always give you the content at the beginning. They should, but they don&#8217;t.
</p>

Did I properly nest headings?
:   Heading order should descend properly, like an outline. So `<h2>` can follow `<h1>` but `<h3>` can&#8217;t. (However, `<h2>` can follow `<h3>`, just like an outline.)

Do I only have one `<h1>` element?
:   This is slightly flexible, but in general you should never have more than two `<h1>` elements. I use `<h1>` for the site or page title, and only once on a page.

Did I create an outline before creating a layout?
:   If your outline doesn&#8217;t make sense, your layout won&#8217;t make sense. This is true for all users. But in particular, without style sheets, your outline is your only page structure.

Do I use semantic names?
:   As [I&#8217;ve said before][10], semantic names provide meaningful structure to code. `<div id="right_column">` means nothing with style sheets turned off. `<div id="content">` or `<div id="user_info">`, however, are still meaningful. Semantic names help you organize your content for all users, and this organization is, again, particularly important without style sheets.

### Validate It.

This checklist is pretty short, but it would have been much longer if I hadn&#8217;t worked so hard on it.

By answering &#8220;yes&#8221; to all the questions on this checklist, you should be in a position to easily pass any of these validators. I&#8217;m not promising: your site may have other issues that I don&#8217;t address here. But with this starting point, it should only take a few minutes to fix any problems that arise with the validators.

Just click on through to any of these, which can help you validate your site.

  * [Web Accessibility Test][11] at Tawdis.net. Pretty good, but no Section 508.
  * [CynthiaSays][12]. Lists all the checkpoints but doesn&#8217;t actually check many of them.
  * [WAVE][13] from WebAIM. Very pretty, does a decent job, but doesn&#8217;t list checkpoints or tell you which standards it uses.
  * [Functional Accessibility Evaluator][14] from the University of Illinois, Urbana-Champaign. Pretty good, but lists their own checkpoints instead of the standard ones.

Read the rest of the series:

  * [Universal Accessibility][15]
  * [What is Universal Accessibility? (Part One in a Trilogy)][16]
  * [Assessing Accessibility (Part Two in a Trilogy)][17]
  * **Building Accessible Sites (Part Three in a Trilogy)**

 [1]: http://coffeeonthekeyboard.com/assessing-accessibility-part-two-in-a-trilogy-67/ "all of the tests"
 [2]: http://coffeeonthekeyboard.com/assessing-accessibility-part-two-in-a-trilogy-67/ "part two"
 [3]: http://validator.w3.org "Use the W3C Validation Services."
 [4]: http://jigsaw.w3.org/css-validator/ "Really, use the W3C Validation Services."
 [5]: javascript:; "This isn't a real link, just a bad example."
 [6]: javascript:; "This isn't a real link, just a good example."
 [7]: javascript:; "This isn't a real link, just a better example."
 [8]: http://www.kryogenix.org/code/browser/lir/ "This is a good image replacement technique."
 [9]: http://www.stopdesign.com/articles/css/replace-text/ "Fahrner Image Replacement, which breaks screen readers."
 [10]: http://coffeeonthekeyboard.com/use-semantics-to-guide-design-53/ "I&#8217;ve said before"
 [11]: http://www.tawdis.net/taw3/cms/en
 [12]: http://www.cynthiasays.com/
 [13]: http://wave.webaim.org/
 [14]: http://fae.cita.uiuc.edu/
 [15]: http://coffeeonthekeyboard.com/universal-accessibility-64/ "Universal Accessibility"
 [16]: http://coffeeonthekeyboard.com/what-is-universal-accessibility-part-one-in-a-trilogy-66/ "What is Universal Accessibility? (Part One in a Trilogy)"
 [17]: http://coffeeonthekeyboard.com/assessing-accessibility-part-two-in-a-trilogy-67/ "Assessing Accessibility (Part Two in a Trilogy)"