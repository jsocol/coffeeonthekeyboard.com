---
title: 'JavaScript: Private Static Members, Part 1'
author: James
layout: post
permalink: /javascript-private-static-members-part-1-208/
bitly_url:
  - http://j0.is/14dZz3F
bitly_hash:
  - 14dZz3F
bitly_long_url:
  - http://coffeeonthekeyboard.com/javascript-private-static-members-part-1-208/
categories:
  - Articles
tags:
  - Code
  - javascript
  - oop
  - programming
  - tutorial
---
[A little while ago][1] I talked about creating private variables and methods in JavaScript. This works, but is not necessarily efficient: each instance of the class creates new copies of the members. While that may be exactly what you want for instance variables (think of `partNum` in the old examples) it is not always ideal.

The complexity jumps significantly, though. So I&#8217;m dividing this half into two parts.

To get started, we need to forget about all this Object-Oriented Programming for a minute and look at some of the neat [tricks][2] you can do with functions in JavaScript.

**Update:** [Part 2][3] is now available.<!--more-->

First, let&#8217;s take a look at a few ways to define a function in JavaScript:

<div class="dean_ch" style="white-space: wrap;">
  <ol>
    <li class="li1">
      <div class="de1">
        <span class="kw2">function</span> oneFunction <span class="br0">&#40;</span><span class="br0">&#41;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        <span class="br0">&#123;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; <span class="co1">// function body goes here</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        <span class="br0">&#125;</span>
      </div>
    </li>
    
    <li class="li2">
      <div class="de2">
        &nbsp;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        <span class="kw2">var</span> anotherFunction = <span class="kw2">function</span> <span class="br0">&#40;</span><span class="br0">&#41;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        <span class="br0">&#123;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; <span class="co1">// function body goes here</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        <span class="br0">&#125;</span>;
      </div>
    </li>
  </ol>
</div>

The first example, `oneFunction` should be familiar to programmers from most languages. The second one is completely equivalent, but works slightly differently. In this case, the right-hand side, a function, is being assigned to the left-hand side, the var `anotherFunction`.

Remember that in JavaScript, functions are first-class objects, just like everything else, so can be declared with the `var` keyword. They can also be passed to other functions as arguments, or returned from functions.

Now let&#8217;s take a brief look at parentheses. What do parentheses really do? Essentially, they evaluate and return whatever expression is inside them. For example:

<div class="dean_ch" style="white-space: wrap;">
  <ol>
    <li class="li1">
      <div class="de1">
        <span class="kw2">var</span> five = <span class="br0">&#40;</span><span class="nu0">5</span><span class="br0">&#41;</span>;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        <span class="co1">// the expression is "5"</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        <span class="kw2">var</span> nine = <span class="br0">&#40;</span><span class="nu0">2</span> * <span class="nu0">4</span><span class="br0">&#41;</span> + <span class="nu0">1</span>;
      </div>
    </li>
    
    <li class="li2">
      <div class="de2">
        <span class="co1">// "2 * 4" is evaluated and returned as "8"</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        <span class="kw2">var</span> nottrue = <span class="br0">&#40;</span><span class="kw2">true</span> || <span class="kw2">false</span><span class="br0">&#41;</span> && <span class="kw2">false</span>;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        <span class="co1">// "true || false" evalutes to "true"</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp;
      </div>
    </li>
    
    <li class="li2">
      <div class="de2">
        <span class="kw2">var</span> thirty = <span class="br0">&#40;</span><span class="br0">&#40;</span><span class="nu0">5</span>*<span class="nu0">5</span><span class="br0">&#41;</span><span class="nu0">-10</span><span class="br0">&#41;</span>*<span class="nu0">2</span>;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        <span class="co1">// "5*5" is evaluated, then returned as 25 to 25-10,</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        <span class="co1">// which evaluates to 15, which is returned and doubled</span>
      </div>
    </li>
  </ol>
</div>

So parentheses are slightly more powerful than the simple grouping operation we associate with them. Sometimes we see examples like this, which may be more illustrative:

<div class="dean_ch" style="white-space: wrap;">
  <ol>
    <li class="li1">
      <div class="de1">
        <span class="kw2">function</span> checkName <span class="br0">&#40;</span><span class="kw3">name</span><span class="br0">&#41;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        <span class="br0">&#123;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp;<span class="kw1">return</span> <span class="br0">&#40;</span><span class="kw3">name</span>==<span class="st0">&#8216;admin&#8217;</span><span class="br0">&#41;</span>;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        <span class="br0">&#125;</span>
      </div>
    </li>
  </ol>
</div>

So what happens if we combine parentheses&#8217; ability to evaluate and return code with our ability to define functions as an expression?

<div class="dean_ch" style="white-space: wrap;">
  <ol>
    <li class="li1">
      <div class="de1">
        <span class="kw2">var</span> aFunc = <span class="br0">&#40;</span><span class="kw2">function</span> <span class="br0">&#40;</span><span class="br0">&#41;</span> <span class="br0">&#123;</span> <span class="coMULTI">/*&#8230;*/</span> <span class="br0">&#125;</span><span class="br0">&#41;</span>;
      </div>
    </li>
  </ol>
</div>

Of course, this is just the same as `anotherFunction` above, but you can see that the right-hand side &#8220;returns&#8221; a function. Let&#8217;s do something a little different now:

<div class="dean_ch" style="white-space: wrap;">
  <ol>
    <li class="li1">
      <div class="de1">
        <span class="br0">&#40;</span><span class="kw2">function</span> <span class="br0">&#40;</span><span class="kw3">name</span><span class="br0">&#41;</span> <span class="br0">&#123;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        <span class="kw3">alert</span><span class="br0">&#40;</span><span class="st0">"Hello, "</span>+<span class="kw3">name</span>+<span class="st0">"!"</span><span class="br0">&#41;</span>;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        <span class="br0">&#125;</span><span class="br0">&#41;</span><span class="br0">&#40;</span><span class="st0">"World"</span><span class="br0">&#41;</span>;
      </div>
    </li>
  </ol>
</div>

What&#8217;s going on here? The first set of parentheses [`(function ... )`] evaluate and return the code inside, creating a function. The last set [`("World")`] are then *calling* the function created by the first set. Immediately.

This is a powerful technique, but has certain limits. The interior function is executed immediately on creation, which means the DOM will probably not be loaded yet. Once the function is executed, it is lost. Trying to save it in the left-hand side of an equation will only save the *return value* of the function, not the function itself. For example:

<div class="dean_ch" style="white-space: wrap;">
  <ol>
    <li class="li1">
      <div class="de1">
        <span class="kw2">var</span> aFunc = <span class="br0">&#40;</span><span class="kw2">function</span> <span class="br0">&#40;</span><span class="br0">&#41;</span> <span class="br0">&#123;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; <span class="kw1">return</span> <span class="st0">"I&#8217;m not a function!"</span>;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        <span class="br0">&#125;</span><span class="br0">&#41;</span><span class="br0">&#40;</span><span class="br0">&#41;</span>;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        <span class="kw3">alert</span><span class="br0">&#40;</span>aFunc<span class="br0">&#41;</span>; <span class="co1">// Alerts "I&#8217;m not a function!"</span>
      </div>
    </li>
  </ol>
</div>

But, remember what I said about functions as first-class objects? It means we can use one function as a return value from another function:

<div class="dean_ch" style="white-space: wrap;">
  <ol>
    <li class="li1">
      <div class="de1">
        <span class="kw2">var</span> bFunc = <span class="br0">&#40;</span><span class="kw2">function</span> <span class="br0">&#40;</span><span class="br0">&#41;</span> <span class="br0">&#123;</span> <span class="co1">// 1</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; <span class="kw1">return</span> <span class="kw2">function</span> <span class="br0">&#40;</span><span class="br0">&#41;</span> <span class="br0">&#123;</span> <span class="co1">// 2</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw3">alert</span><span class="br0">&#40;</span><span class="st0">"Hello, World!"</span><span class="br0">&#41;</span>;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; <span class="br0">&#125;</span>;
      </div>
    </li>
    
    <li class="li2">
      <div class="de2">
        <span class="br0">&#125;</span><span class="br0">&#41;</span><span class="br0">&#40;</span><span class="br0">&#41;</span>; <span class="co1">// 3</span>
      </div>
    </li>
  </ol>
</div>

The outer function (1) is executed immediately (3), and the var `bFunc` stores its return value, which is the inner function (2). So now, `bFunc` is a function, and calling `bFunc()` will alert &#8220;Hello, World!&#8221;.

We&#8217;ll stop for now. If this technique is new to you, play with it for a while. If not, just hang tight and I&#8217;ll get to [Part 2][3] soon enough.

 [1]: http://coffeeonthekeyboard.com/private-variables-in-javascript-177/ "A little while ago"
 [2]: http://coffeeonthekeyboard.com/firefox-open-in-blank-tab-197/ "tricks"
 [3]: http://coffeeonthekeyboard.com/javascript-private-static-members-part-2-218/ "Part 2"