---
title: 'JavaScript: Private Static Members, Part 2'
author: James
layout: post
permalink: /javascript-private-static-members-part-2-218/
bitly_url:
  - http://j0.is/14dZwVC
bitly_hash:
  - 14dZwVC
bitly_long_url:
  - http://coffeeonthekeyboard.com/javascript-private-static-members-part-2-218/
categories:
  - Articles
tags:
  - Code
  - Front-end
  - javascript
---
Finally, it&#8217;s time to finish up the lesson on private static members and methods in JavaScript.

[Last time][1], I introduced the technique of creating and immediately executing a function, using parentheses. I talked a little about *returning* a function and storing it in a variable.

<div class="dean_ch" style="white-space: wrap;">
  <ol>
    <li class="li1">
      <div class="de1">
        <span class="kw2">var</span> myFunc = <span class="br0">&#40;</span><span class="kw2">function</span> <span class="br0">&#40;</span><span class="br0">&#41;</span> <span class="br0">&#123;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; <span class="kw1">return</span> <span class="kw2">function</span> <span class="br0">&#40;</span><span class="br0">&#41;</span> <span class="br0">&#123;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; <span class="kw3">alert</span><span class="br0">&#40;</span><span class="st0">"Hello, World!"</span><span class="br0">&#41;</span>;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; <span class="br0">&#125;</span>
      </div>
    </li>
    
    <li class="li2">
      <div class="de2">
        <span class="br0">&#125;</span><span class="br0">&#41;</span><span class="br0">&#40;</span><span class="br0">&#41;</span>;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        <span class="kw3">alert</span><span class="br0">&#40;</span>myFunc<span class="br0">&#41;</span>; <span class="co1">// "function () &#8230; "</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        myFunc<span class="br0">&#40;</span><span class="br0">&#41;</span>; <span class="co1">// Hello, World!</span>
      </div>
    </li>
  </ol>
</div>

<!--more-->

  
There are a lot of things you can do with this trick, like create interesting [bookmarklets][2]. But let&#8217;s see how you can use it to protect information on the class level.

Here we&#8217;ll take advantage of JavaScript&#8217;s scope behavior. Remember that a function uses the variables where it is *defined*, not executed. Perhaps a better example&#8230;

<div class="dean_ch" style="white-space: wrap;">
  <ol>
    <li class="li1">
      <div class="de1">
        <span class="kw2">var</span> myFunc = <span class="br0">&#40;</span><span class="kw2">function</span> <span class="br0">&#40;</span><span class="br0">&#41;</span> <span class="br0">&#123;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; <span class="kw2">var</span> message = <span class="st0">"I&#8217;m hidden."</span>;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; <span class="kw1">return</span> <span class="kw2">function</span> <span class="br0">&#40;</span><span class="br0">&#41;</span> <span class="br0">&#123;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; <span class="kw3">alert</span><span class="br0">&#40;</span>message<span class="br0">&#41;</span>;
      </div>
    </li>
    
    <li class="li2">
      <div class="de2">
        &nbsp; <span class="br0">&#125;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        <span class="br0">&#125;</span><span class="br0">&#41;</span><span class="br0">&#40;</span><span class="br0">&#41;</span>;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        <span class="kw2">var</span> message = <span class="st0">"I&#8217;m visible."</span>;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp;
      </div>
    </li>
    
    <li class="li2">
      <div class="de2">
        myFunc<span class="br0">&#40;</span><span class="br0">&#41;</span>; <span class="co1">// "I&#8217;m hidden."</span>
      </div>
    </li>
  </ol>
</div>

We can see that the inner function (which is returned from the outer function and set to `myFunc`) uses the value of `message` from the block where it was defined, not executed.

Now you should be able to see where this is going. Let&#8217;s look at a more complex example, extended from the [first part][3]:

<div class="dean_ch" style="white-space: wrap;">
  <ol>
    <li class="li1">
      <div class="de1">
        <span class="kw2">var</span> Product = <span class="br0">&#40;</span><span class="kw2">function</span> <span class="br0">&#40;</span><span class="br0">&#41;</span> <span class="br0">&#123;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; <span class="kw2">var</span> partNumRegex = <span class="re0">/^\d<span class="br0">&#123;</span><span class="nu0">4</span><span class="br0">&#125;</span>\-\d<span class="br0">&#123;</span><span class="nu0">2</span><span class="br0">&#125;</span>$/</span>;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; <span class="kw1">return</span> <span class="kw2">function</span> <span class="br0">&#40;</span> num <span class="br0">&#41;</span>
      </div>
    </li>
    
    <li class="li2">
      <div class="de2">
        &nbsp; &nbsp; <span class="br0">&#123;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw2">var</span> partNum = <span class="kw2">null</span>;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw1">this</span>.<span class="me1">setPartNum</span> = <span class="kw2">function</span> <span class="br0">&#40;</span> n <span class="br0">&#41;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; &nbsp; &nbsp; <span class="br0">&#123;</span>
      </div>
    </li>
    
    <li class="li2">
      <div class="de2">
        &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw1">if</span> <span class="br0">&#40;</span>partNumRegex.<span class="me1">test</span><span class="br0">&#40;</span>n<span class="br0">&#41;</span><span class="br0">&#41;</span> <span class="br0">&#123;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; partNum = n;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw1">return</span> <span class="kw2">true</span>;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="br0">&#125;</span> <span class="kw1">else</span> <span class="br0">&#123;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw1">return</span> <span class="kw2">false</span>;
      </div>
    </li>
    
    <li class="li2">
      <div class="de2">
        &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="br0">&#125;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; &nbsp; &nbsp; <span class="br0">&#125;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw1">this</span>.<span class="me1">getPartNum</span> = <span class="kw2">function</span> <span class="br0">&#40;</span><span class="br0">&#41;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; &nbsp; &nbsp; <span class="br0">&#123;</span>
      </div>
    </li>
    
    <li class="li2">
      <div class="de2">
        &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw1">return</span> partNum;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; &nbsp; &nbsp; <span class="br0">&#125;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw1">if</span> <span class="br0">&#40;</span>num<span class="br0">&#41;</span> <span class="kw1">this</span>.<span class="me1">setPartNum</span><span class="br0">&#40;</span>num<span class="br0">&#41;</span>;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp;
      </div>
    </li>
    
    <li class="li2">
      <div class="de2">
        &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw1">return</span> <span class="kw1">this</span>;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; <span class="br0">&#125;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        <span class="br0">&#125;</span><span class="br0">&#41;</span><span class="br0">&#40;</span><span class="br0">&#41;</span>;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        <span class="kw2">var</span> car = <span class="kw2">new</span> Product;
      </div>
    </li>
    
    <li class="li2">
      <div class="de2">
        car.<span class="me1">setPartNum</span><span class="br0">&#40;</span><span class="st0">&#8216;1234-56&#8242;</span><span class="br0">&#41;</span>;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        <span class="kw2">var</span> table = <span class="kw2">new</span> Product;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        table.<span class="me1">setPartNum</span><span class="br0">&#40;</span><span class="st0">&#8216;345678&#8217;</span><span class="br0">&#41;</span>;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp;
      </div>
    </li>
    
    <li class="li2">
      <div class="de2">
        <span class="kw3">alert</span><span class="br0">&#40;</span><span class="st0">"Car: "</span>+car.<span class="me1">getPartNum</span><span class="br0">&#40;</span><span class="br0">&#41;</span><span class="br0">&#41;</span>; <span class="co1">// "Car: 1234-56"</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        <span class="kw3">alert</span><span class="br0">&#40;</span><span class="st0">"Table: "</span>+table.<span class="me1">getPartNum</span><span class="br0">&#40;</span><span class="br0">&#41;</span><span class="br0">&#41;</span>; <span class="co1">// "Table: null"</span>
      </div>
    </li>
  </ol>
</div>

What&#8217;s the advantage here? The variable `partNumRegex` is *not* copied by the `new` operator. In a small example like this, there is not much benefit, but if you had hundreds of `Product` objects, you could save a significant amount of memory.

There are a few major drawbacks: a public static (class) method cannot access a private static method or variable. For example:

<div class="dean_ch" style="white-space: wrap;">
  <ol>
    <li class="li1">
      <div class="de1">
        <span class="kw2">var</span> Product = <span class="br0">&#40;</span><span class="kw2">function</span> <span class="br0">&#40;</span><span class="br0">&#41;</span> <span class="br0">&#123;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; <span class="kw2">var</span> partNumRegex = <span class="re0">/^\d<span class="br0">&#123;</span><span class="nu0">4</span><span class="br0">&#125;</span>\-\d<span class="br0">&#123;</span><span class="nu0">2</span><span class="br0">&#125;</span>$/</span>;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; <span class="kw1">return</span> <span class="kw2">function</span> <span class="br0">&#40;</span><span class="br0">&#41;</span> <span class="br0">&#123;</span>
      </div>
    </li>
    
    <li class="li2">
      <div class="de2">
        &nbsp; &nbsp; &nbsp; &nbsp; <span class="co1">// snip</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; <span class="br0">&#125;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        <span class="br0">&#125;</span><span class="br0">&#41;</span><span class="br0">&#40;</span><span class="br0">&#41;</span>;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        Product.<span class="me1">validPartNum</span> = <span class="kw2">function</span> <span class="br0">&#40;</span>num<span class="br0">&#41;</span> <span class="br0">&#123;</span>
      </div>
    </li>
    
    <li class="li2">
      <div class="de2">
        &nbsp; &nbsp; <span class="kw1">if</span> <span class="br0">&#40;</span>partNumRegex.<span class="me1">test</span><span class="br0">&#40;</span>num<span class="br0">&#41;</span><span class="br0">&#41;</span> <span class="br0">&#123;</span> <span class="co1">// (1)</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw1">return</span> <span class="kw2">true</span>;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; <span class="br0">&#125;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; <span class="kw1">return</span> <span class="kw2">false</span>;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        <span class="br0">&#125;</span>
      </div>
    </li>
  </ol>
</div>

The class method `validPartNum` has no access to the private class variable `partNumRegex`, and so will throw an error at (1). Adding an accessor *must* be done on the *instance*, not the class, like so:

<div class="dean_ch" style="white-space: wrap;">
  <ol>
    <li class="li1">
      <div class="de1">
        <span class="kw2">var</span> Product = <span class="br0">&#40;</span><span class="kw2">function</span> <span class="br0">&#40;</span><span class="br0">&#41;</span> <span class="br0">&#123;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; <span class="kw2">var</span> partNumRegex = <span class="re0">/^\d<span class="br0">&#123;</span><span class="nu0">4</span><span class="br0">&#125;</span>\-\d<span class="br0">&#123;</span><span class="nu0">2</span><span class="br0">&#125;</span>$/</span>;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; <span class="kw1">return</span> <span class="kw2">function</span> <span class="br0">&#40;</span><span class="br0">&#41;</span> <span class="br0">&#123;</span>
      </div>
    </li>
    
    <li class="li2">
      <div class="de2">
        &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw1">this</span>.<span class="me1">getPartNumRegex</span> = <span class="kw2">function</span> <span class="br0">&#40;</span><span class="br0">&#41;</span> <span class="br0">&#123;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw1">return</span> partNumRegex;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; &nbsp; &nbsp; <span class="br0">&#125;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; &nbsp; &nbsp; <span class="co1">// snip</span>
      </div>
    </li>
    
    <li class="li2">
      <div class="de2">
        &nbsp; &nbsp; <span class="br0">&#125;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        <span class="br0">&#125;</span><span class="br0">&#41;</span><span class="br0">&#40;</span><span class="br0">&#41;</span>;
      </div>
    </li>
  </ol>
</div>

But then you cannot access the private variable without first creating an instance of the class, and the accessor function is copied with the `new` operator. New methods added to the `Product.prototype` object are likewise unable to access the private static variables. This is a limitation of JavaScript.

Even with these limitations, the ability to hide implementation behind an agreed-upon interface is powerful. (JavaScript doesn&#8217;t actually have interfaces, but you can just write it down.) Behind the scenes, you could load new data via Ajax, without ever exposing your Ajax method to that new guy down the hall who likes to misuse everything he can:

<div class="dean_ch" style="white-space: wrap;">
  <ol>
    <li class="li1">
      <div class="de1">
        <span class="kw2">var</span> Product = <span class="br0">&#40;</span><span class="kw2">function</span> <span class="br0">&#40;</span><span class="br0">&#41;</span> <span class="br0">&#123;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; <span class="kw2">var</span> partNumRegex = <span class="re0">/^\d<span class="br0">&#123;</span><span class="nu0">4</span><span class="br0">&#125;</span>\-\d<span class="br0">&#123;</span><span class="nu0">2</span><span class="br0">&#125;</span>$/</span>;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; <span class="co1">// private static function, not copied with "new"</span>
      </div>
    </li>
    
    <li class="li2">
      <div class="de2">
        &nbsp; &nbsp; <span class="kw2">function</span> loadPartData<span class="br0">&#40;</span>partNum<span class="br0">&#41;</span> <span class="br0">&#123;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; &nbsp; &nbsp; <span class="co1">// load data via Ajax</span>
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
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; <span class="kw1">return</span> <span class="kw2">function</span> <span class="br0">&#40;</span><span class="br0">&#41;</span> <span class="br0">&#123;</span>
      </div>
    </li>
    
    <li class="li2">
      <div class="de2">
        &nbsp;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw2">var</span> partNum = <span class="kw2">null</span>;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; &nbsp; &nbsp; <span class="co1">// snip</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp;
      </div>
    </li>
    
    <li class="li2">
      <div class="de2">
        &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw1">this</span>.<span class="me1">setPartNum</span><span class="br0">&#40;</span>num<span class="br0">&#41;</span> <span class="br0">&#123;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw1">if</span><span class="br0">&#40;</span>partNumRegex.<span class="me1">test</span><span class="br0">&#40;</span>num<span class="br0">&#41;</span><span class="br0">&#41;</span> <span class="br0">&#123;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; partNum = num;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw2">var</span> data = loadPartData<span class="br0">&#40;</span>partNum<span class="br0">&#41;</span>;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw1">this</span>.<span class="me1">productName</span> = data.<span class="me1">productName</span>;
      </div>
    </li>
    
    <li class="li2">
      <div class="de2">
        &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw1">this</span>.<span class="me1">price</span> = data.<span class="me1">price</span>;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw1">return</span> <span class="kw2">true</span>;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="br0">&#125;</span> <span class="kw1">else</span> <span class="br0">&#123;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw1">return</span> <span class="kw2">false</span>;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span class="br0">&#125;</span>
      </div>
    </li>
    
    <li class="li2">
      <div class="de2">
        &nbsp; &nbsp; &nbsp; &nbsp; <span class="br0">&#125;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; &nbsp; &nbsp; <span class="co1">// snip</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; <span class="br0">&#125;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        <span class="br0">&#125;</span><span class="br0">&#41;</span><span class="br0">&#40;</span><span class="br0">&#41;</span>;
      </div>
    </li>
  </ol>
</div>

That&#8217;s it for now. I owe most of these three articles to the book [Pro JavaScript Design Patterns][4], by Ross Harmes and Dustin Diaz. Those two are geniuses, and anyone who wants to be a better JavaScript programmer would do well to pick up their book.

Next up, I&#8217;ll argue why the `<dl>` tag is a good way to display forms semantically.

 [1]: http://coffeeonthekeyboard.com/javascript-private-static-members-part-1-208/
 [2]: http://coffeeonthekeyboard.com/firefox-open-in-blank-tab-197/
 [3]: http://coffeeonthekeyboard.com/private-variables-in-javascript-177/
 [4]: http://www.amazon.com/JavaScript-Design-Patterns-Recipes-Problem-Solution/dp/159059908X