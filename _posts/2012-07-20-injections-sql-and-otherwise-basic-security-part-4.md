---
title: 'Injections, SQL and otherwise &#8211; Basic Security Part 4'
author: James
layout: post
permalink: /injections-sql-and-otherwise-basic-security-part-4-755/
bitly_url:
  - http://j0.is/14dZEEy
bitly_hash:
  - 14dZEEy
bitly_long_url:
  - http://coffeeonthekeyboard.com/injections-sql-and-otherwise-basic-security-part-4-755/
categories:
  - Security
tags:
  - django
  - django-nyc-security
  - injection
  - mozilla
  - security
  - sql
---
*NB: This is the fourth post in a [series][1] of posts on web application security.*

## SQL Injection

SQL injection is a vector that lets a user insert their own SQL into a statement sent to your database server. The typical example is:

<div class="dean_ch" style="white-space: wrap;">
  <ol>
    <li class="li1">
      <div class="de1">
        <span class="st0">"SELECT * FROM users WHERE username = &#8216;"</span> + username + <span class="st0">"&#8217; AND <span class="es0">\</span></span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        <span class="st0">password = &#8216;"</span> + password <span class="st0">&#8216;"</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        <span class="st0"</span>
      </div>
    </li>
  </ol>
</div>

Then someone submits `' OR 1=1; --` as their password.

To avoid this, use parameterized queries or, better yet, use framework-level functions like ORMs that make it very difficult to do this wrong. In Django, use the [auth framework][2], but even if you wanted to look up users for another reason, [use the ORM][3]:

<div class="dean_ch" style="white-space: wrap;">
  <ol>
    <li class="li1">
      <div class="de1">
        User.<span class="me1">objects</span>.<span class="me1">get</span><span class="br0">&#40;</span>username=username<span class="br0">&#41;</span>
      </div>
    </li>
  </ol>
</div>

Sometimes ORMs don&#8217;t produce the fastest queries, or we want to do complicated joins. That&#8217;s fine, there are still tools. Django provides the [Queryset.raw()][4] method. Use it correctly:

<div class="dean_ch" style="white-space: wrap;">
  <ol>
    <li class="li1">
      <div class="de1">
        User.<span class="me1">objects</span>.<span class="me1">raw</span><span class="br0">&#40;</span><span class="st0">&#8216;SELECT username FROM auth_user WHERE username=%s&#8217;</span>,
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;<span class="br0">&#91;</span>username<span class="br0">&#93;</span><span class="br0">&#41;</span>
      </div>
    </li>
  </ol>
</div>

## Other types of Injections

SQL injection is only one type of injection. Much worse are injections that let users run code on your server. PHP&#8217;s [backtick passthrough][5] and Python&#8217;s `subprocess.call` are both dangerous places that need to be carefully audited. If possible, they should be avoided completely. If that&#8217;s not possible, don&#8217;t put user data in there. `eval()` is another risky thing to avoid if at all possible.

In Python, the [`ast.literal_eval`][6] method is both safe and more often than not does whatever it was you thought you needed `eval()` to do in the first place.

If you&#8217;re running code on the server under any user, even the web process user&mdash;which should have almost no permissions&mdash;don&#8217;t put user data in there. Just don&#8217;t do it. Find another way.

  * Go to the [series index][7].
  * Go to the previous post in the series, [CSRF: Cross-Site Request Forgeries][8].
  * Go to the next post in the series, [Access Control][9].

 [1]: http://coffeeonthekeyboard.com/best-basic-security-practices-especially-with-django-697/
 [2]: https://docs.djangoproject.com/en/dev/topics/auth/
 [3]: https://docs.djangoproject.com/en/dev/topics/db/queries/
 [4]: https://docs.djangoproject.com/en/dev/topics/db/sql/#passing-parameters-into-raw
 [5]: http://php.net/manual/en/language.operators.execution.php
 [6]: http://docs.python.org/library/ast.html#ast.literal_eval
 [7]: http://coffeeonthekeyboard.com/best-basic-security-practices-especially-with-django-697/ "Best Basic Security Practices (Especially with Django)"
 [8]: http://coffeeonthekeyboard.com/csrf-cross-site-request-forgeries-basic-security-part-3-747/ "CSRF: Cross-Site Request Forgeries – Basic Security Part 3"
 [9]: http://coffeeonthekeyboard.com/access-control-basic-security-part-5-765/ "Access Control – Basic Security Part 5"