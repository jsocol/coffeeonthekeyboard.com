---
title: 'SUMO&#8217;s Black Triangle'
author: James
layout: post
permalink: /sumos-black-triangle-371/
bitly_url:
  - http://j0.is/14dZJYP
bitly_hash:
  - 14dZJYP
bitly_long_url:
  - http://coffeeonthekeyboard.com/sumos-black-triangle-371/
dsq_thread_id:
  - 1746519901
categories:
  - Articles
tags:
  - black triangle
  - mozilla
  - sumo
  - webdev
---
A &#8220;[black triangle][1]&#8221; is an accomplishment or milestone that doesn&#8217;t look like much—it may be literally a black triangle on a screen—but is the result of a lot of preliminary or foundational work.

A black triangle often means that the pace of work is about to increase dramatically.

The phrase, for example, comes from drawing a black triangle on a monitor. It wasn&#8217;t just a triangle:

> It was the journey the triangle had taken to get up on the screen. It had passed through our new modeling tools, through two different intermediate converter programs, had been loaded up as a complete database, and been rendered through a fairly complex scene hierarchy, fully textured and lit (though there were no lights, so the triangle came out looking black). The black triangle demonstrated that the foundation was finally complete – the core of a fairly complex system was completed, and we were now ready to put it to work doing cool stuff. By the end of the day, we had complete models on the screen, manipulating them with the controllers. Within a week, we had an environment to move the model through.

So what does a black triangle in a video game have to do with SUMO?

Like the game developers, we&#8217;re starting on a new system, and the new system needs a foundation. My first black triangle moment with [Kitsune][2] was a plain white screen with some page names on it. Those names were search results, and they meant that I had [Django][3] running, finding and running my code, connecting to [Sphinx][4], collating results,  connecting to the database, getting the data, and displaying them.

It didn&#8217;t look impressive, but it was the culmination of a lot more work than you might think.

Today I merged in code that redirects from `/search` to `/en-US/search`. In a way, it&#8217;s also a black triangle. That redirect requires us to have everything from a list of languages we support to our own middleware that is capable of understanding locales at the beginnings of URLs, or in the query string, or from the HTTP Accept-Language header, and where to send you in each case. It also needs to know how to generate URLs with the correct locale, and then there were all the tests that had to be written and to pass. For just 6 characters.

But those six characters are part of the foundation the new platform. All of our interface is localizable, and when we get to rewriting the knowledge base, those six characters will be crucial.

Paul has been working on code&#8211;quite a lot of code&#8211;that can tell if you&#8217;re logged in to SUMO. This is no small feat: we&#8217;re maintaining consistent data between sessions in applications written in two completely different languages. But the result is so small you might not even notice. All you&#8217;ll see is a &#8220;log out&#8221; link instead of a &#8220;log in&#8221; link when using the search, if you&#8217;re logged in.

But in the next milestone, that session information will be an integral part of building our discussion forum.

Black triangles are incredibly exciting for the developers, because we know exactly what they are. We know what&#8217;s going on under the hood, what work went in to making them happen, and what it means we&#8217;re ready to start.

The [first commit][5] to Kitsune was nearly two months ago, and right now, we&#8217;re polishing up something that looks nearly identical to the thing it will replace. But along the way we&#8217;ve built a ton of functionality that is going to let us start building the next part, and the next part will be much more visible.

 [1]: http://rampantgames.com/blog/2004/10/black-triangle.html
 [2]: https://wiki.mozilla.org/Support/Kitsune
 [3]: http://www.djangoproject.com/
 [4]: http://www.sphinxsearch.com/
 [5]: http://github.com/jsocol/kitsune/commit/2c0472cf890b9dbfec76fc9d97baf0a7220a60d1