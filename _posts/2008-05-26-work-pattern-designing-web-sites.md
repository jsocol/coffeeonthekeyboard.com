---
title: 'Work Pattern: Designing Web Sites'
author: James
layout: post
permalink: /work-pattern-designing-web-sites-93/
bitly_url:
  - http://j0.is/16FvnDu
bitly_hash:
  - 16FvnDu
bitly_long_url:
  - http://coffeeonthekeyboard.com/work-pattern-designing-web-sites-93/
dsq_thread_id:
  - 1746519829
categories:
  - Articles
  - Design
tags:
  - Accessibility
  - Code
  - CSS
  - Design
  - html
  - Standards
  - work pattern
  - workflow
  - xhtml
---
The premise of [Design Patterns][1] is that similar problems have similar solutions. In the same vein, I propose this **Work Pattern** a set of common steps I use when I create a web site, and maybe you can use, too.

#### Elements and Outline

My first step is usually to create an un-styled outline of a &#8220;typical&#8221; page. I fire up my editor, fill in the basic XHTML, and then go to work inside the `<body>` tag.

Most sites have this fairly common structure: header, content, footer. And just for fun, let&#8217;s throw in navigation between the header and the content. It&#8217;s pretty easy to represent this in XHTML:

<pre>&lt;div id="header"&gt;
&lt;/div&gt;

&lt;div id="navigation"&gt;
&lt;/div&gt;

&lt;div id="content"&gt;
&lt;/div&gt;

&lt;div id="footer"&gt;
&lt;/div&gt;</pre>

This is my first skeleton for >90% of the sites I design. It&#8217;s a very standard document. Sometimes navigation will be inside the header, but most often it goes like this.

Now you have to start thinking about what elements will be on the page. On this site, a blog, I used &#8220;articles&#8221; instead of &#8220;content&#8221; for the main div. I also added two side bars, and I knew that inside the articles div I&#8217;d want, well, articles.

<pre>&lt;div id="header"&gt;
  &lt;h1&gt;Page Title&lt;/h1&gt;
&lt;/div&gt;

&lt;div id="navigation"&gt;
  &lt;ul&gt;
    &lt;li&gt;Link 1&lt;/li&gt;
    &lt;li&gt;Link 2&lt;/li&gt;
  &lt;/ul&gt;
&lt;/div&gt;

&lt;div id="articles"&gt;
  &lt;h2&gt;Recent Articles&lt;/h2&gt;
  &lt;div class="article"&gt;
    &lt;h3&gt;Article Title&lt;/h3&gt;
  &lt;/div&gt;
&lt;/div&gt;

&lt;div id="theblog"&gt;
  &lt;h2&gt;Sidebar heading&lt;/h2&gt;
  &lt;p&gt;Sidebar paragraph&lt;/p&gt;
&lt;/div&gt;

&lt;div id="theworld"&gt;
  &lt;h2&gt;Sidebar heading&lt;/h2&gt;
  &lt;ul&gt;
    &lt;li&gt;Sidebar&lt;/li&gt;
    &lt;li&gt;list&lt;/li&gt;
  &lt;/ul&gt;
&lt;/div&gt;

&lt;div id="footer"&gt;
&lt;/div&gt;</pre>

<p class="image right">
  <img src="http://coffeeonthekeyboard.com/wp-content/uploads/2008/05/skeleton.png" alt="An Un-Styled Skeleton" />
</p>

I won&#8217;t bore you with more code examples; I think you get the idea. I [make an outline][2]. I know at this point that my source is nice and valid, and that it will make sense when I [turn off the stylesheet][3]. I use [semantic names][4] for everything.

It&#8217;s not very pretty, but I now have a workable XHTML document, with a properly-nested outline, and most of the important elements. Good for me, because now I can start to style them.

#### Layout and Style

Now, I know what visual elements will need to go on the page. I know what page elements I need to style. Now I&#8217;ll start creating a style sheet.

My first style sheet will contain a few basic <abbr>HTML tags and the elements of my document. I could probably write an XML</abbr>-to-CSS generator with how strict I am with this step.

Ok, one more code example:

<pre>body {}

h1,
h2,
h3,
h4,
h5,
h6 {}

a:link {}
a:visited {}
a:hover {}

#header {}
#header h1 {}

#navigation {}
#navigation ul {}
#navigation ul li {}

#articles {}
#articles h2 {}
#articles div.article {}
#articles div.article h2 {}

#theblog {}
#theblog h2 {}

#theworld {}
#theworld h2 {}
#theworld ul {}
#theworld ul li {}

#footer {}</pre>

One of my favorite things about this is it&#8217;s almost impossible for a mistake in one section to mess up anything else.

But obviously there&#8217;s a lot in there I can combine, can shorten. Almost anything that&#8217;s true for `#theblog` will also be true for `#theworld` in this case, so DRY, and keep things together as much as you can. But, when you&#8217;re just starting the style sheet, this is a good place to start.

As I&#8217;m going, I add a lot to the style sheet. I also add a lot to the XHTML template. Pixels get tweaked left and right and I swear at IE6, of course.

#### Building Templates

Once I have a complete, or near-complete, [mock up][5], it&#8217;s time to start building templates for your CMS of choice. This is mostly copy-and-paste work at this point. Your `#header` and `#navigation` go into the header template. `#footer` goes into footer. `#content` goes in the content template.

See how easy that is?

Then you get to go through and actually add the template mark up. Whether it&#8217;s Smarty or PHP or ASP doesn&#8217;t really matter, you just replace your dummy text with the right tags.

#### Starting Out

I love this process, but there is one thing you really need for it to go smoothly:

You need to know what kind of content you&#8217;ll have. When you&#8217;re redesigning your blog, or building an in-house site, it&#8217;s pretty easy to know. When you&#8217;re working for a client, you may need to twist some arms to get this information. (I love [this A List Apart article][6] for advice on communicating with clients.)

One final thought: use comments. Any time I create a div, I wrap it in comments like this:

<pre>&lt;!--begin #articles --&gt;
&lt;div id="articles"&gt;
&lt;/div&gt;
&lt;!-- end #articles --&gt;</pre>

I usually use the CSS selector because it&#8217;s specific, so `#articles`, `.article`, and so on. These comments—which I left out here to save space—have saved me so much time and effort compared to relying on indentation that I can&#8217;t imagine working without them.

I didn&#8217;t set out this process as a way to streamline my work, but rather, as I started noticing patterns that worked well, I started thinking about the process. Much like [Rails][7], which was already running [Basecamp][8] before it was a framework, I&#8217;ve been using more-and-more-polished versions of this work flow for months.

Maybe you&#8217;ll find it helpful, maybe not. Maybe you already have a &#8220;system&#8221; in place. If you do, what is it?

 [1]: http://en.wikipedia.org/wiki/Design_pattern_(computer_science)
 [2]: http://coffeeonthekeyboard.com/building-accessible-sites-part-three-in-a-trilogy-69/ "make an outline"
 [3]: http://coffeeonthekeyboard.com/assessing-accessibility-part-two-in-a-trilogy-67/ "turn off the stylesheet"
 [4]: http://coffeeonthekeyboard.com/use-semantics-to-guide-design-53/ "semantic names"
 [5]: http://coffeeonthekeyboard.com/wp-content/themes/mock.htm
 [6]: http://www.alistapart.com/articles/designbymetaphor
 [7]: http://rubyonrails.org/
 [8]: http://www.basecamphq.com/