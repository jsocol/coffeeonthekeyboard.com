---
title: Reservations about HTML5
author: James
layout: post
permalink: /reservations-about-html5-271/
bitly_url:
  - http://j0.is/zUa7o
bitly_hash:
  - zUa7o
bitly_long_url:
  - http://coffeeonthekeyboard.com/reservations-about-html5-271/
dsq_thread_id:
  - 1746519031
categories:
  - Articles
tags:
  - extensibility
  - html5
  - role
  - spec
  - suggestions
  - whatwg
  - xhtml5
---
A day or so ago, [Adrian Bateman][1], from [Microsoft&#8217;s IE team][2], posted his team&#8217;s [thoughts on the current draft][3] of the [HTML5 spec][4].

Reading it is brutal. Bateman takes issue with basically everything added since HTML4. He goes through and individually criticizes many of the new tags, sometimes with extremely detailed, multi-paragraph critiques. I guess this is what happens when you&#8217;re not sufficiently involved at the beginning.

Of course, there is still plenty of time to complain, since [the HTML5 spec won&#8217;t reach its final stage until 2022][5].

Bateman and the IE team, even while sounding like they don&#8217;t even *want* HTML5, do bring up a few things that have been bothering me about the spec.

Let me be very clear: I think new tags like `<audio>` and `<video>` are wonderful. Breaking the Adobe monopoly is great. There are still some issues (refusing to specify a codec meaning you can&#8217;t build support into the browser, for instance) but those types of tags are going to help push the web to a better place.

The parts that bother me are the new, highly touted &#8220;[structural][6]&#8221; tags, like `<header>`, `<footer>`, `<section>`, and worst, `<article>` and `<aside>`.<!--more-->

My issue is that all of these tags are perfect semantic additions, *if all you want to do is put magazine articles on the web*. These are tags that represent a *current* prevailing paradigm in *text-focused* web design that has been strongly influenced by *print design and layout*. They all have a use on [A List Apart][7], and on most blogs, but besides `<header>`—or possibly `<nav>`—can you see any of them in [Gmail ][8]or [Google Calendar][9]?

[Ryan Doherty][10] already demonstrated a [semantic version of Gmail][11], and didn&#8217;t need a single HTML5 tag to do it. (But please, *please*, don&#8217;t assume I&#8217;m putting words in Ryan&#8217;s mouth about HTML5. He&#8217;s also used the `<audio>` tag to [great effect][12].)

Issues with [the ambiguity of certain new tags][13] aside, these tags privilege, even codify, a certain paradigm in design. For lack of a better word, the &#8220;wall of text&#8221; style. To that end, I worry that (a) they do nothing to help the cause of <del datetime="2009-08-08T20:49:35+00:00">emerging</del> <ins datetime="2009-08-08T20:49:35+00:00">hell already mainstream</ins> web application development—the [Rich Internet Application][14]— and (b) they may *actively discourage* designers from trying new, paradigm breaking ideas.

I imagine a conversation between a designer and the developer tasked with implementing the design. Maybe this isn&#8217;t a &#8220;great&#8221; developer, but honestly, how many of those are there? The conversation ends: &#8220;Well, there just isn&#8217;t a tag for that.&#8221; Back to the drawing board.

What I like about the `<div>` and `<span>` are their flexibility. By themselves, they&#8217;re nearly meaningless, just a block-level or inline element, respectively. What makes them work is the way we&#8217;ve developed the use of the `id` and `class` attributes.

What HTML5&#8217;s new tags lack is *extensibility*, just like HTML4. They freeze us at a moment in time (about 5 years ago) and in design. To that end, I reiterate [John Allsopp][15]&#8216;s [call for new attributes][16], like [`role` from XHTML][17], rather than new tags. Do we really need five new block-level elements? Or should we allow some sort of extensible mechanism to create, in effect, an infinite number of new block-level elements?

Since I suggested it, here&#8217;s how I imagine it working: let&#8217;s say HTML5 adopts the `role` attribute, and it&#8217;s applicable to everything. Now, instead of XHTML&#8217;s short list of values for `role`, let&#8217;s define a list of values, but allow that list to be extended. So, our common values—like `header`, `footer`, `navigation`, `menu`, `section`, `article`, `frame`, `search`, `banner`, posssibly `data` and `interactive`, I&#8217;m sure we could come up with far more even now—have some defined meaning, but they are not the only possible values.

The tricky part is: how do we go about defining a *new* role? The simple, naïve solution is allowing people to use anything, and then use CSS selectors and JavaScript to make it work. While this opens up the extensibility to the greatest number of people, it also makes it fairly meaningless, and doesn&#8217;t really expand on the `id`/`class` system we have now.

The other end of the spectrum is requiring all roles to have a definition that is somehow meaningful to the user-agent. The XML way to do this would be roughly to allow the role attribute to exist in any namespace and still be interpreted, providing a definition for the UA somewhere on the web. Something like this:

<pre>&lt;html ... u:xmlns="http://my.dtd/url"&gt;
...
&lt;div u:role="mycustomrole" role="interactive"&gt;</pre>

This, of course, limits the number of people who can create, and host, definitions for roles. It is also incompatible with the HTML-style variant of HTML5 (as opposed to XHTML5, the XML-style variant). So let&#8217;s throw this out now.

The other method I can see off the top of my head is a hybrid, and puts much of the onus on the UA to be smart, and on the community to share.

There will need to be a lot of discussion among browser vendors on what &#8220;meaningful to the UA&#8221; means, and what kind of definitions are necessary from that, but let&#8217;s assume that they&#8217;ve all come to some sort of consensus—or, since I&#8217;ve always thought the consensus model was flawed, that some common format has emerged. There is a file format that can be placed on the web.

This is a hybrid scheme: if all I want is to label some elements with a new role, I can put anything in there. But if I want something meaningful to the UA, with a definition file, I can add it as a `<link>` element in the header.

<pre>&lt;link rel="roles" type="application/role+xml" href="http://mysite.com/myroles"/&gt;</pre>

<p class="aside">
  MIME-types and formats are left as exercises to the reader.
</p>

Now, here&#8217;s where the sharing comes in. Let&#8217;s say my friend or follower wants to use a similar role, and thinks mine is close enough. He can then put the same `<link/>` tag. Hopefully, a smart UA, would have cached the definition file, do a conditional `GET` request, get a `304 Not Modified` from my server. If my definition file gets too popular for my server to handle, I could perhaps move it over to Google Code and have my server issue a `301 Moved Permanently`, which would hopefully convince UAs to stop pinging me.

There is some question about conflicting role definitions. What makes a &#8220;unique key,&#8221; is it a combination of a role and a URL? Can I define multiple roles per URL, or only one? If there are multiple roles per URL, can I, as a developer, mix and match? Which definition would take precedence? These are not trivial questions, and would have to go up for debate in [WHATWG][18].

This is just one idea for an extensible method of building on HTML that doesn&#8217;t lock us into a single design paradigm. What I don&#8217;t like about the current HTML5 spec is that a lot of is is neither extensible nor forward-thinking. I don&#8217;t know how many of you plan to keep making the same old blogs and webzines for the next 13 years, but I, for one, would like to move forward.

 [1]: http://adrianba.net/
 [2]: http://blogs.msdn.com/ie/default.aspx
 [3]: http://bit.ly/1TNzYU
 [4]: http://dev.w3.org/html5/spec/Overview.html
 [5]: http://www.webmonkey.com/blog/HTML_5_Won_t_Be_Ready_Until_2022DOT_Yes__2022DOT
 [6]: http://microformatique.com/?p=83
 [7]: http://www.alistapart.com/
 [8]: http://gmail.com
 [9]: http://www.google.com/calendar
 [10]: http://www.ryandoherty.net/
 [11]: http://www.ryandoherty.net/2009/03/23/426/
 [12]: http://ryandoherty.net/clouserwsoundboard/
 [13]: http://www.zeldman.com/2009/07/13/html-5-nav-ambiguity-resolved/
 [14]: http://en.wikipedia.org/wiki/Rich_Internet_application
 [15]: http://microformatique.com/
 [16]: http://www.alistapart.com/articles/semanticsinhtml5
 [17]: http://www.w3.org/TR/xhtml-role/
 [18]: http://www.whatwg.org/