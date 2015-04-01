---
title: 'Access Control &#8211; Basic Security Part 5'
author: James
layout: post
permalink: /access-control-basic-security-part-5-765/
bitly_url:
  - http://j0.is/16AFpUF
bitly_hash:
  - 16AFpUF
bitly_long_url:
  - http://coffeeonthekeyboard.com/access-control-basic-security-part-5-765/
dsq_thread_id:
  - 1746518908
categories:
  - Security
tags:
  - access control
  - django
  - django-nyc-security
  - mozilla
  - security
---
*NB: This is the fifth post in a [series][1] of posts on web application security.*

Proper access control is an absolutely key part of web app security and is easily overlooked&mdash;possibly because it&#8217;s so easy. In Django, to hide a link from someone, you just:

<div class="dean_ch" style="white-space: wrap;">
  <ol>
    <li class="li1">
      <div class="de1">
        <span class="br0">&#123;</span>% <a href="http://smarty.php.net/if"><span class="kw1">if</span></a> perms.<span class="me1">myapp</span>.<span class="me1">mymodel</span> %<span class="br0">&#125;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; <a href="<span class="br0">&#123;</span>% url myapp:mymodel_edit obj.<span class="me1">pk</span> %<span class="br0">&#125;</span>">Edit</a>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        <span class="br0">&#123;</span>% endif %<span class="br0">&#125;</span>
      </div>
    </li>
  </ol>
</div>

Hiding a link isn&#8217;t enough, though, you need to make sure that privileged pages are only accessible to the right set of users.

In Django, use the [`@permission_required`][2] decorator, and it&#8217;ll pretty much handle everything for you:

<div class="dean_ch" style="white-space: wrap;">
  <ol>
    <li class="li1">
      <div class="de1">
        @permission_required<span class="br0">&#40;</span><span class="st0">&#8216;myapp.change_mymodel&#8217;</span><span class="br0">&#41;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        <span class="kw1">def</span> edit_thing<span class="br0">&#40;</span>request, obj_id<span class="br0">&#41;</span>:
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; <span class="st0">""</span><span class="st0">"Edit a MyObject."</span><span class="st0">""</span>
      </div>
    </li>
  </ol>
</div>

There are also [`@login_required`][3] and the more complex [`@user_passes_test`][4] decorators, helping provide a whole spectrum of authentication tools.

[Read the docs][5]. Use the tools. You&#8217;ll be fine.

  * Go to the [series index][6].
  * Go to the previous post in the series, [Injections, SQL and otherwise][7].
  * Go to the next post in the series, [Session Fixation and Hijacking][8].

 [1]: http://coffeeonthekeyboard.com/best-basic-security-practices-especially-with-django-697/
 [2]: https://docs.djangoproject.com/en/dev/topics/auth/#the-permission-required-decorator
 [3]: https://docs.djangoproject.com/en/dev/topics/auth/#the-login-required-decorator
 [4]: https://docs.djangoproject.com/en/dev/topics/auth/#django.contrib.auth.decorators.user_passes_test
 [5]: https://docs.djangoproject.com/en/dev/topics/auth/#authentication-in-web-requests
 [6]: http://coffeeonthekeyboard.com/best-basic-security-practices-especially-with-django-697/ "Best Basic Security Practices (Especially with Django)"
 [7]: http://coffeeonthekeyboard.com/injections-sql-and-otherwise-basic-security-part-4-755/ "Injections, SQL and otherwise – Basic Security Part 4"
 [8]: http://coffeeonthekeyboard.com/session-fixation-and-hijacking-basic-security-part-6-801/ "Session Fixation and Hijacking – Basic Security Part 6"