---
title: 'Mass Assignment &#8211; Security Part 10'
author: James
layout: post
permalink: /mass-assignment-security-part-10-855/
bitly_url:
  - http://j0.is/14e00uR
bitly_hash:
  - 14e00uR
bitly_long_url:
  - http://coffeeonthekeyboard.com/mass-assignment-security-part-10-855/
categories:
  - Security
tags:
  - django
  - django-nyc-security
  - forms
  - models
  - mozilla
  - security
---
*NB: This is the tenth post in a [series][1] of posts on web application security.*

> &#8220;Mass assignment&#8221;? That&#8217;s a [Rails][2] thing! 

[GitHub][3] was the recent, high-profile target of an &#8220;attack&#8221;&mdash;it wasn&#8217;t so much a vicious attack as a &#8220;hey you guys, this is serious&#8221; attack, really [gray-hat at its darkest][4]&mdash;that made use of a *feature* in Rails called [Mass Assignment][5].

So why, in a series of posts ostensibly about [Django][6], am I talking about a feature in Rails?

Because Mass Assignment, and the underlying vulnerability, boil down to a lack of *whitelisting*, and that&#8217;s something that any application, Rails, Django, or otherwise, can be susceptible to. You want to limit what your users can change to *what they&#8217;re allowed to change*.

Imagine a Django developer who did something along the lines of this:



Fortunately, that doesn&#8217;t quite work in the same way. Django has some built-in protections, but you can see there are obviously some risks involved, especially in the `update` example, since we&#8217;re operating on a `QuerySet` instead of an object itself, and have access to an `update()` method.

The correct way to do this sort of thing in Django is to use [Forms][7]. There are always going to be special cases and weird requirements, but forms are *almost always* the right thing to do. The trick is just to use them the right way. Compare these two examples:



This is a step in the right direction. This creates a form that does a bunch of basic validation on the types of data allowed. But really, it doesn&#8217;t do much beyond what the model validates itself. At the very least, you&#8217;ll want to do something like this:



This *whitelists* the `fields` the user can change. (There is also an `excludes` property that let&#8217;s you blacklist fields. Don&#8217;t use it, prefer `fields`.)

Now that you&#8217;ve got your `ModelForm` instance, you can add all sorts of custom validation logic. Django gives you &#8220;your age must be an integer.&#8221; You can insist &#8220;and you need to be at least 21.&#8221; Then when you when you call the built-in `is_valid()` method, the form will run all of this validation for you, and only let users change what you let them, to what you let them. A very, very simple example:



If there is anything you take from this post, let it be these 4 simple things:

  1. Django isn&#8217;t quite as susceptible to mass assignment as Rails, but that doesn&#8217;t mean you&#8217;re in the clear.
  2. Read the [Django forms docs][7] and use them.
  3. **Use the *fields* property** to whitelist what can be changed.
  4. Forms may seem like boilerplate but they&#8217;re a maintenance win in the long run. Especially when you start adding custom validation.

And just be careful anytime you let someone create or modify objects&mdash;this goes for anyone in any app in any framework. Use whitelists, not blacklists, and certainly not nothing, to decide who can change what.

Next up, we&#8217;ll get to some interesting issues from my own experiences here. This [intermission][8] won&#8217;t be quite as long, I promise.

  * Go to the [series index][9].
  * Go to the previous post in the series, [Stay Up to Date][10].

 [1]: http://coffeeonthekeyboard.com/best-basic-security-practices-especially-with-django-697/
 [2]: http://rubyonrails.org/
 [3]: https://github.com/
 [4]: https://github.com/blog/1068-public-key-security-vulnerability-and-mitigation
 [5]: http://guides.rubyonrails.org/security.html#mass-assignment
 [6]: https://www.djangoproject.com/
 [7]: https://docs.djangoproject.com/en/dev/topics/forms/
 [8]: http://coffeeonthekeyboard.com/intermission-848/ "Intermission"
 [9]: http://coffeeonthekeyboard.com/best-basic-security-practices-especially-with-django-697/ "Best Basic Security Practices (Especially with Django)"
 [10]: http://coffeeonthekeyboard.com/stay-up-to-date-basic-security-part-9-834/ "Stay Up to Date – Basic Security Part 9"