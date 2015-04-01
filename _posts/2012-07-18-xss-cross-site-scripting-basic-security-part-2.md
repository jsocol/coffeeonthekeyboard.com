---
title: 'XSS: Cross-Site Scripting &#8211; Basic Security Part 2'
author: James
layout: post
permalink: /xss-cross-site-scripting-basic-security-part-2-711/
bitly_url:
  - http://j0.is/14e05i5
bitly_hash:
  - 14e05i5
bitly_long_url:
  - http://coffeeonthekeyboard.com/xss-cross-site-scripting-basic-security-part-2-711/
dsq_thread_id:
  - 1746519081
categories:
  - Security
tags:
  - django
  - django-nyc-security
  - mozilla
  - security
---
*NB: This is the second post in a [series][1] of posts on web application security.*

XSS covers a number of various attacks, but the common thread is that someone gets to execute code in the context of your web page and domain. Doing that, they can do all sorts of things, primarily collecting data from logged in users, like session IDs, or worse.

There are a number of things you can do to help minimize the impact of an XSS vector against your site&mdash;and you should do them, because the more complicated the app, the more subtle holes you&#8217;re likely to end up with. We&#8217;ll cover those later, but first, let&#8217;s cover the first level of due diligence. You need to:

  * &#8230;properly escape any user content that ends up in your pages&#8217; HTML.
  * &#8230;not use tag or attribute blacklists.
  * &#8230;audit the source of data that ends up rendered.

Any template language worth using, like [Django][2] or [Jinja2][3], will automatically HTML-escape anything variable you output, unless you take special steps not to. **Don&#8217;t disable automatic escaping.** Anything that needs to let HTML through should be opt-in, so you minimize the *surface area* of attack.

There is an interesting case with localization. For example, you might need to localize the string `Hello, <span>Username</span>`. Localization (L10n for short) 101 says you can&#8217;t do the simple thing:

    _('Hello,') + ' <span>' + escape(username) + '</span>'
    

Because in some locales, the name will need to come first. And you can&#8217;t do the other simple thing, because it&#8217;s insecure:

    _('Hello, <span>%s</span>') % username
    

In fact, in Jinja2, at least, interpolation marks the translated string as *unsafe* and will cause everything, even the `<span>` tags to get escaped.

For cases like this, we built a filter in [Jingo][4] (our Jinja-for-Django template adapter) that escapes input and then interpolates it, marking the final output as safe. An [example from our code base][5], in Jinja2:

{% raw %}
    {{ _('<em>Editing</em> {title}')|fe(title=page.title) }}
{% endraw %}
    

Even if `page.title` has HTML in it, that will be escaped, but the `<em>` tags will not. This is getting into more complicated use cases, but it is possible, and critical, with L10n and other concerns, to default to escaping content.

Sometimes you need to let some HTML through. For example, a blog with comments may want to let commenters use *some* HTML tags. To do this safely, use a white-list approach, and a tool like [Bleach][6].

    comment = bleach.clean(comment, tags=['em', 'strong', 'br'])
    

Bleach will not only escape any tags not in the whitelist, it will also close unbalanced tags so you won&#8217;t have a stray `<b>` or `<div>` ruining your whole page&#8217;s layout.

## On JavaScript

Because I just [fixed][7] [this bug][8], I&#8217;d like to talk about JS a bit.

It&#8217;s easy, even with JS frameworks, to simply put together a string of HTML and add it to the DOM via `innerHTML`. If user data ends up in one of these strings, you can have a problem. For a very simple example:

    var stuff = '<h2>' + username + '</h2>';
    document.getElementById('#subhead').innerHTML = stuff;
    

Now `username` doesn&#8217;t need to be escaped as *JavaScript*, but as *HTML*. Even if it came from the server [packaged safely as a JS string][9], you&#8217;re still creating an XSS vector.

The route I chose, because it was expedient, was essentially to implement Python&#8217;s [`markupsafe.escape`][10] or PHP&#8217;s [`htmlspecialchars()`][11] in JS.

But if you&#8217;re starting from scratch or from a place where you already have a DOM node, you can generally use a JS framework to help you. For example, in [jQuery][12], the [`.text()` method][13] uses the DOM&#8217;s `.createTextNode` and so escapes any special characters:

    $('#subhead h2').text(username);
    

At least in the latest jQuery, that&#8217;s safe.

  * Go to the [series index][14].
  * Go to the previous post in the series, [Password Storage][15].
  * Go to the next post in the series, [CSRF: Cross-Site Request Forgeries][16].

 [1]: http://coffeeonthekeyboard.com/best-basic-security-practices-especially-with-django-697/
 [2]: https://docs.djangoproject.com/en/dev/topics/templates/
 [3]: http://jinja.pocoo.org/
 [4]: https://github.com/jbalogh/jingo
 [5]: https://github.com/mozilla/kitsune/blob/master/apps/wiki/templates/wiki/edit_document.html#L17
 [6]: https://github.com/jsocol/bleach
 [7]: https://github.com/mozilla/kitsune/commit/9416717ed28
 [8]: https://bugzilla.mozilla.org/show_bug.cgi?id=772998
 [9]: http://docs.python.org/library/json.html
 [10]: https://github.com/mitsuhiko/markupsafe/blob/master/markupsafe/_native.py#L14
 [11]: http://php.net/manual/en/function.htmlspecialchars.php
 [12]: http://jquery.com/
 [13]: http://api.jquery.com/text/#text2
 [14]: http://coffeeonthekeyboard.com/best-basic-security-practices-especially-with-django-697/ "Best Basic Security Practices (Especially with Django)"
 [15]: http://coffeeonthekeyboard.com/password-storage-basic-security-part-1-706/ "Password Storage – Basic Security Part 1"
 [16]: http://coffeeonthekeyboard.com/csrf-cross-site-request-forgeries-basic-security-part-3-747/ "CSRF: Cross-Site Request Forgeries – Basic Security Part 3"
