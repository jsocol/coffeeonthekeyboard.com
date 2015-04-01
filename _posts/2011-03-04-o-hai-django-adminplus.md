---
title: O Hai Django AdminPlus
author: James
layout: post
permalink: /o-hai-django-adminplus-568/
bitly_url:
  - http://j0.is/16FvqiI
bitly_hash:
  - 16FvqiI
bitly_long_url:
  - http://coffeeonthekeyboard.com/o-hai-django-adminplus-568/
dsq_thread_id:
  - 1746519190
categories:
  - Articles
tags:
  - django
  - mozilla
  - python
  - webdev
---
Last night, as happens sometimes, I was wishing it was possible to add some of our custom admin views to the Django admin&#8217;s index page. It&#8217;s kind of a pain to have to type the URL every time, especially when talking to other people: &#8220;It&#8217;s in the admin, but no, you can&#8217;t&#8230; just go to this URL.&#8221;

So I was reading the [Django admin docs][1] and, frankly, they aren&#8217;t great. They tell you about the benefits of subclassing `AdminSite`, or running multiple `AdminSite`s, but they never really mention *how to do it*.

So, with a little trial and error, I wrote my `AdminSite` subclass and figured out how to use it. It simplified a bunch of code in our custom admin app, so I thought I would share it.

Here&#8217;s [Django AdminPlus][2] (and [on PyPI][3]).

AdminPlus adds a method, `admin.site.register_view`, that connects an arbitrary view function to your admin, and puts a link to it on the admin index page. If you put calls to `register_view` in `someapp/admin.py`, they&#8217;re picked up during the normal `admin.autodiscover()` process.

<div class="dean_ch" style="white-space: wrap;">
  <ol>
    <li class="li1">
      <div class="de1">
        <span class="co1"># myapp/admin.py</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        <span class="kw1">from</span> django.<span class="me1">contrib</span> <span class="kw1">import</span> admin
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        <span class="kw1">from</span> django.<span class="me1">shortcuts</span> <span class="kw1">import</span> render_to_response
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp;
      </div>
    </li>
    
    <li class="li2">
      <div class="de2">
        <span class="kw1">def</span> my_admin_view<span class="br0">&#40;</span>request<span class="br0">&#41;</span>:
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; <span class="kw1">return</span> render_to_response<span class="br0">&#40;</span><span class="st0">&#8216;myapp/admin/view.html&#8217;</span><span class="br0">&#41;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        admin.<span class="kw3">site</span>.<span class="me1">register_view</span><span class="br0">&#40;</span><span class="st0">&#8216;mypath&#8217;</span>, my_admin_view<span class="br0">&#41;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        <span class="co1"># With an optional &#8216;name&#8217; parameter:</span>
      </div>
    </li>
    
    <li class="li2">
      <div class="de2">
        <span class="kw1">def</span> my_other_admin_view<span class="br0">&#40;</span>request<span class="br0">&#41;</span>:
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; <span class="kw1">return</span> render_to_response<span class="br0">&#40;</span><span class="st0">&#8216;myapp/admin/other.html&#8217;</span><span class="br0">&#41;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        admin.<span class="kw3">site</span>.<span class="me1">register_view</span><span class="br0">&#40;</span><span class="st0">&#8216;otherpath&#8217;</span>, my_other_admin_view, <span class="st0">&#8216;Fancy Stuff!&#8217;</span><span class="br0">&#41;</span>
      </div>
    </li>
  </ol>
</div>

Assuming your admin URLs start with `admin/`, this will make `my_admin_view` available at `admin/mypath` and `my_other_admin_view` available at `admin/otherpath`, but it also puts them in a new section on the admin index, so you don&#8217;t even need to worry about the URL.

If no name is given we try to guess from the name of the view function.

That&#8217;s about it! Read the [README][4] and [the other docs][5] and enjoy!

 [1]: http://docs.djangoproject.com/en/dev/ref/contrib/admin/
 [2]: https://github.com/jsocol/django-adminplus
 [3]: http://pypi.python.org/pypi/django-adminplus
 [4]: https://github.com/jsocol/django-adminplus#readme
 [5]: https://github.com/jsocol/django-adminplus/tree/master/docs