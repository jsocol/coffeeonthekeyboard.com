---
title: Actually Starting an Open Source Project
author: James
layout: post
permalink: /actually-starting-an-open-source-project-907/
bitly_url:
  - http://j0.is/14dZZHx
bitly_hash:
  - 14dZZHx
bitly_long_url:
  - http://coffeeonthekeyboard.com/actually-starting-an-open-source-project-907/
categories:
  - Articles
tags:
  - Code
  - open source
  - Projects
---
I&#8217;m a little late to the party, but I just got around to reading [Starting an Open-Source Project][1] and, as someone who has started several reasonably successful projects, I wanted to publicly disagree with, essentially, the entire article.

The article outlines **seven** pretty big steps to take before you can even consider open-sourcing a project.

  1. Identify your goals
  2. Choose a license
  3. Organize your code
  4. Write documentation
  5. Set up a mailing list
  6. Use version numbers
  7. Organize contributors

Most of those have several substeps. For example, *Organize contributors* includes considering a [CLA][2] and contributor hierarchy.

This is *scary*. I really need to do all that to open source some code? Forget it!

**No, you don&#8217;t.** In my experience there are exactly two things you need to do:

  1. Choose a license
  2. Put the code somewhere!

In fact, some people even get away without #1 (but you shouldn&#8217;t).

Choosing a license can seem daunting, but after you&#8217;ve done it once or twice, you can do it pretty quickly. To me, the most important question is whether you want a [copyleft][3] license or not, after you&#8217;ve decided that, there are a few options, but the details matter less. And if there&#8217;s a community standard, you can generally use that (e.g. [BSD-3][4] for Django apps).

Then just get the code out there. Honestly.

If you *just know* that you&#8217;re releasing a huge, successful project, maybe it&#8217;s worth putting in more of the effort up front. But that knowledge is pretty rare.

Here are a few examples.

## [OOCurl][5]

I wrote this 5 years ago because I was annoyed with the [PHP cURL][6] bindings. I stuck an [MIT license][7] in the source and put it up on my website and a public SVN repo.

As far as I know, no one used it. That&#8217;s OK. Maybe someone did? I don&#8217;t know.

Then a few years ago I shut down my SVN server and moved all the projects to GitHub. I had no intent to, as the article says &#8220;continue developing the code,&#8221; and that&#8217;s also fine. With a project like that, you have two options: put it somewhere public or don&#8217;t. If you don&#8217;t, odds are you will lose it over time. If you do, maybe someone finds it and finds it useful, maybe they don&#8217;t.

Since putting it on GitHub, I&#8217;ve gotten a few patches from folks, so I know it&#8217;s out there. It&#8217;s helping people, and I skipped six of their steps!

## [Bleach][8]

Bleach was always intended to be open source. I don&#8217;t think that affected very much at the outset, though. Pre-1.0 versions are pretty rough (especially pre-0.3) but it was always open, on GitHub and up on PyPI.

Since it was useful and solved a problem, people stumbled onto it. GitHub and PyPI have weight with Google, so just being there and having a half-decent description of what the code does was enough. I also tweeted about it from time to time, but who even reads Twitter?

I point out Bleach because it has all the goals the article mentions (it&#8217;s useful to the world, is under active development, and accepts contributions) and all of that happened without any thought to the rest of their steps. And now it&#8217;s a reasonably successful project, well-known within the Python web community.

## [Waffle][9]

I literally woke up one Saturday morning, thought to myself &#8220;hey, I bet that would work,&#8221; and wrote the core of Waffle in a couple of hours. And put it on GitHub with a BSD3 license.

Now, it&#8217;s shockingly (to me, anyway) popular, and gets contributions from all over the place.

## (Humble) Bragging?

The point of all this isn&#8217;t to brag, it&#8217;s to point out that you can start a perfectly successful open source project with almost no effort.

Later, after you&#8217;ve established that something is useful, to you, at least, start talking about it more, maybe set up more of the infrastructure. It depends on how the community of contributors grows.

Don&#8217;t sweat the other stuff. **Write code. Stick a license on it. Put it out there.**

The rest can all come later, if it needs to.

 [1]: http://coding.smashingmagazine.com/2013/01/03/starting-open-source-project/
 [2]: http://en.wikipedia.org/wiki/Contributor_License_Agreement
 [3]: http://www.gnu.org/copyleft/
 [4]: http://opensource.org/licenses/BSD-3-Clause
 [5]: https://github.com/jsocol/oocurl
 [6]: http://php.net/manual/en/book.curl.php
 [7]: http://opensource.org/licenses/MIT
 [8]: https://github.com/jsocol/bleach
 [9]: https://github.com/jsocol/django-waffle/