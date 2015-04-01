---
title: Bleach, HTML sanitizer and auto-linker
author: James
layout: post
permalink: /bleach-html-sanitizer-and-auto-linker-for-django-344/
bitly_url:
  - http://j0.is/16FvvD5
bitly_hash:
  - 16FvvD5
bitly_long_url:
  - http://coffeeonthekeyboard.com/bleach-html-sanitizer-and-auto-linker-for-django-344/
dsq_thread_id:
  - 1747402768
categories:
  - Articles
tags:
  - data
  - django
  - html
  - mozilla
  - python
  - sanitize
  - security
  - user
---
[Bleach][1] is a whitelist-based HTML sanitizer and auto-linker in Python, built on [html5lib][2], for [AMO][3] and [SUMO][4] and released under the BSD license.

Bleach has two main functions: sanitizing HTML based on a whitelist of tags and attributes, and turning URLs into links. It uses html5lib for both.

For more information on using Bleach, see the [README][5] included in the source. For more info on how Bleach works, follow below the jump.<!--more-->

### Sanitizing HTML

Bleach&#8217;s `clean()` function uses a slightly custom version of html5lib&#8217;s `HTMLSanitizer` tokenizer that adds support for per-tag attribute whitelists. Any entity that is not part of a whitelisted tag or valid entity will be encoded. Legitimate entities and tags are allowed. The default whitelist is set up for AMO.

### Linkifying Text

The `linkify()` function is a little more complicated. Naïve implementations usually rely on a simple regular expression to find URL-like strings, but this quickly becomes insufficient when you need to handle situations like these:

  * `<em>http://example.com</em>` (should be linkified)
  * `<a href="http://example.com">test</a>` (already linked, no need to linkify)
  * `<a href="http://example.com">http://example.com</a>` (really don&#8217;t need to linkify)
  * `<em>http://xx.com <a href="http://example.com">http://example.com</a></em>` (regular expression freak-out)

So `linkify()` actually uses html5lib to build a document fragment and walks it, only applying the naïve regular expression in safe locations. In pseudocode:

<div class="dean_ch" style="white-space: wrap;">
  <ol>
    <li class="li1">
      <div class="de1">
        tree = parseFragment<span class="br0">&#40;</span><span class="kw2">input</span><span class="br0">&#41;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        linkify_nodes <span class="br0">&#40;</span>tree<span class="br0">&#41;</span>:
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; <span class="kw1">for</span> node <span class="kw1">in</span> tree:
      </div>
    </li>
    
    <li class="li2">
      <div class="de2">
        &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw1">if</span> node <span class="kw1">is</span> a text node:
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; replace node with text nodes <span class="kw1">and</span> links
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw1">else</span> <span class="kw1">if</span> node <span class="kw1">is</span> a link:
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw1">if</span> nofollow:
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw2">set</span> rel=<span class="st0">"nofollow"</span> on node
      </div>
    </li>
    
    <li class="li2">
      <div class="de2">
        &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw1">else</span>:
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; linkify_nodes<span class="br0">&#40;</span>node.<span class="me1">childNodes</span><span class="br0">&#41;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        <span class="kw1">return</span> <span class="kw3">string</span><span class="br0">&#40;</span>linkify_nodes<span class="br0">&#40;</span>tree<span class="br0">&#41;</span><span class="br0">&#41;</span>
      </div>
    </li>
  </ol>
</div>

This avoids attempting to apply the regular expression to things like tag attributes, the inside of `<a>` tags, and other places it should generally be avoided. It also lets us do things like set the `rel` attribute on links already in the text and pass the `href` attribute through the same filter it would go through if we created the link. This filter lets us redirect links through an outbound redirect, so people know they&#8217;re leaving a Mozilla site. You could do other things with it, like rickroll your visitors. That&#8217;s up to you.

### Bad HTML

Because both `clean()` and `linkify()` use `html5lib` and construct document trees, using either will fix up code mistakes, like unclosed takes, and escape bare entities. `linkify()` allows basically every tag and attribute, so if you need to limit the legal HTML to a subset, use `clean()` (or the shortcut `bleach()` to clean then linkify).

### Getting Bleach

Bleach is [available on Github][1], or can be installed via `pip` or `easy_install`. Improvements and test cases are very welcome! Actually, there&#8217;s one disabled test right now that is not supported. If you can make it work, that would be pretty great!

 [1]: http://github.com/jsocol/bleach
 [2]: http://code.google.com/p/html5lib/
 [3]: https://addons.mozilla.org/
 [4]: http://support.mozilla.com/
 [5]: http://github.com/jsocol/bleach/blob/master/README.rst