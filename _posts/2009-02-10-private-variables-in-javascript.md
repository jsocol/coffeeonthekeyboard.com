---
title: Private Variables in JavaScript
author: James
layout: post
permalink: /private-variables-in-javascript-177/
bitly_url:
  - http://j0.is/14dZyNm
bitly_hash:
  - 14dZyNm
bitly_long_url:
  - http://coffeeonthekeyboard.com/private-variables-in-javascript-177/
categories:
  - Articles
tags:
  - client-side
  - javascript
  - objects
  - programming
---
<p class="note">
  Ok, enough of this social/ranting stuff. Time to write something vaguely technical.
</p>

I have a love-hate relationship with [JavaScript][1]. I think anyone who works with it does. Sometimes it just doesn&#8217;t do what you expect, and it&#8217;s certainly different.

One trick, especially for people from real Object-Oriented languages like Java, Ruby, or let&#8217;s even say PHP 5, is the lack of access control. When everything is an object, the inability to hide certain values can become a problem.<!--more-->

Say, for example, you create an object that represents a product you sell, and you want to use one of your part numbers to identify the object. Let&#8217;s say the format for your part number is 1234-56. You might do something like&#8230;

<div class="dean_ch" style="white-space: wrap;">
  <ol>
    <li class="li1">
      <div class="de1">
        <span class="kw2">function</span> Product <span class="br0">&#40;</span><span class="br0">&#41;</span> <span class="br0">&#123;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; <span class="kw1">this</span>.<span class="me1">partNum</span>;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; <span class="kw1">this</span>.<span class="me1">setPartNum</span> = <span class="kw2">function</span> <span class="br0">&#40;</span> num <span class="br0">&#41;</span> <span class="br0">&#123;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw1">if</span> <span class="br0">&#40;</span> partNumRegex.<span class="me1">match</span><span class="br0">&#40;</span>num<span class="br0">&#41;</span> <span class="br0">&#41;</span> <span class="br0">&#123;</span>
      </div>
    </li>
    
    <li class="li2">
      <div class="de2">
        &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw1">this</span>.<span class="me1">partNum</span> = num;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw1">return</span> <span class="kw2">true</span>;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; &nbsp; &nbsp; <span class="br0">&#125;</span> <span class="kw1">else</span> <span class="br0">&#123;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw1">return</span> <span class="kw2">false</span>;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; &nbsp; &nbsp; <span class="br0">&#125;</span>
      </div>
    </li>
    
    <li class="li2">
      <div class="de2">
        &nbsp; &nbsp; <span class="br0">&#125;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        <span class="br0">&#125;</span>
      </div>
    </li>
  </ol>
</div>

You&#8217;re expecting the rest of your team to be respectful and use the `Product.setPartNum()` method, which should stop them from using an illegally formatted part number. (`partNumRegex` is left as an exercise to the reader.)

But what&#8217;s stopping them from doing this?

<div class="dean_ch" style="white-space: wrap;">
  <ol>
    <li class="li1">
      <div class="de1">
        <span class="kw2">var</span> jacket = <span class="kw2">new</span> Product;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        jacket.<span class="me1">partNum</span> = <span class="st0">&#8216;1234&#8217;</span>;
      </div>
    </li>
  </ol>
</div>

Now when they call your Ajax-driven `getProductData()` method, they&#8217;ll get an error. Had they used the `setPartNum()` method, they would&#8217;ve seen the error much earlier.

One trick to JavaScript is that variables are [lexically scoped][2]. Basically, functions use the value of a variable where they are defined, not where they&#8217;re executed, for example (or just read [a better article][3]):

<div class="dean_ch" style="white-space: wrap;">
  <ol>
    <li class="li1">
      <div class="de1">
        <span class="kw2">var</span> a = <span class="nu0">1</span>;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        <span class="kw2">function</span> globalA<span class="br0">&#40;</span><span class="br0">&#41;</span> <span class="br0">&#123;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; <span class="kw3">alert</span><span class="br0">&#40;</span>a<span class="br0">&#41;</span>; <span class="co1">//1</span>
      </div>
    </li>
    
    <li class="li2">
      <div class="de2">
        <span class="br0">&#125;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        <span class="kw2">function</span> localA <span class="br0">&#40;</span><span class="br0">&#41;</span> <span class="br0">&#123;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; <span class="kw2">var</span> a = <span class="nu0">2</span>;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; <span class="kw3">alert</span><span class="br0">&#40;</span>a<span class="br0">&#41;</span>; <span class="co1">//2</span>
      </div>
    </li>
    
    <li class="li2">
      <div class="de2">
        <span class="br0">&#125;</span>
      </div>
    </li>
  </ol>
</div>

So how does this help? Well, you can effectively hide values in a deeper scope. Instead of using the `this` keyword, just define new variables within your function:

<div class="dean_ch" style="white-space: wrap;">
  <ol>
    <li class="li1">
      <div class="de1">
        <span class="kw2">function</span> Product <span class="br0">&#40;</span><span class="br0">&#41;</span> <span class="br0">&#123;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; <span class="kw2">var</span> partNum;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; <span class="kw1">this</span>.<span class="me1">setPartNum</span> = <span class="kw2">function</span> <span class="br0">&#40;</span>num<span class="br0">&#41;</span> <span class="br0">&#123;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; &nbsp; &nbsp; <span class="co1">// Fancy checking logic</span>
      </div>
    </li>
    
    <li class="li2">
      <div class="de2">
        &nbsp; &nbsp; <span class="br0">&#125;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; <span class="kw1">this</span>.<span class="me1">getPartNum</span> = <span class="kw2">function</span> <span class="br0">&#40;</span><span class="br0">&#41;</span> <span class="br0">&#123;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw1">return</span> partNum;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; <span class="br0">&#125;</span>
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
        <span class="kw2">var</span> tshirt = <span class="kw2">new</span> Product;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        tshirt.<span class="me1">partNum</span> = <span class="nu0">7</span>;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        tshirt.<span class="me1">getPartNum</span><span class="br0">&#40;</span><span class="br0">&#41;</span>; <span class="co1">// null</span>
      </div>
    </li>
  </ol>
</div>

This same thing can apply to private methods. Don&#8217;t want the new guy calling `superSecretInternalMethod()`? One common practice is to start private methods with an underscore, but a better way is to actually hide the method:

<div class="dean_ch" style="white-space: wrap;">
  <ol>
    <li class="li1">
      <div class="de1">
        <span class="kw2">function</span> Product<span class="br0">&#40;</span><span class="br0">&#41;</span> <span class="br0">&#123;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; <span class="kw2">var</span> partNum; <span class="co1">// private part number, use accessors</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; <span class="kw1">this</span>.<span class="me1">setPartNum</span> = <span class="kw2">function</span><span class="br0">&#40;</span><span class="br0">&#41;</span><span class="br0">&#123;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; &nbsp; &nbsp; <span class="co1">// as above</span>
      </div>
    </li>
    
    <li class="li2">
      <div class="de2">
        &nbsp; &nbsp; <span class="br0">&#125;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; <span class="kw1">this</span>.<span class="me1">getPartNum</span> = <span class="kw2">function</span><span class="br0">&#40;</span><span class="br0">&#41;</span> <span class="br0">&#123;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw1">return</span> partNum;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; <span class="br0">&#125;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp;
      </div>
    </li>
    
    <li class="li2">
      <div class="de2">
        &nbsp; &nbsp; <span class="kw2">function</span> superSecretInternalMethod<span class="br0">&#40;</span><span class="br0">&#41;</span> <span class="br0">&#123;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; &nbsp; &nbsp; <span class="co1">// Whatever your coding-ninja heart desires</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; &nbsp; &nbsp; <span class="co1">// I can access partNum</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; <span class="br0">&#125;</span>
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
        <span class="kw2">var</span> truck = <span class="kw2">new</span> Product;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        truck.<span class="me1">superSecretInternalMethod</span><span class="br0">&#40;</span><span class="br0">&#41;</span>; <span class="co1">// JS Error: truck.superSecretInternalMethod is not a function.</span>
      </div>
    </li>
  </ol>
</div>

We can even go a step farther and define private class methods and variables, but this post is already long enough, and it&#8217;s rare I have material for two in a row, so I&#8217;ll save that for next time.

<p class="note">
  If you haven&#8217;t read it, go get <a href="http://www.amazon.com/JavaScript-Design-Patterns-Recipes-Problem-Solution/dp/159059908X">Pro JavaScript Design Patterns</a>. You won&#8217;t regret it.
</p>

 [1]: http://www.quirksmode.org/dom/compatibility.html
 [2]: http://en.wikipedia.org/wiki/Scope_(programming)#Static_scoping_.28also_known_as_lexical_scoping.29
 [3]: http://blogs.msdn.com/kartikb/archive/2009/01/15/lexical-scoping.aspx