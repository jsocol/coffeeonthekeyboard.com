---
title: Introducing Waffle for Django
author: James
layout: post
permalink: /introducing-waffle-for-django-541/
bitly_url:
  - http://j0.is/16FvqiH
bitly_hash:
  - 16FvqiH
bitly_long_url:
  - http://coffeeonthekeyboard.com/introducing-waffle-for-django-541/
dsq_thread_id:
  - 1746519583
categories:
  - Articles
tags:
  - django
  - mozilla
  - python
  - webdev
---
**Waffle** is a feature-flipping library for **Django** that strives to be easy and intuitive, and work with both the Django/Jingo/Jinja2 stack we use at Mozilla, and Django templates out of the box.

Waffle lets you define various reasons a **flag** can be active for a given request. You can make flags active for all superusers, or authenticated users, for example. You can also define a percentage of &#8220;everyone else&#8221; who will see the flag as active.

If Waffle uses a dice roll to determine if the current request will have the flag turned on, it sets a **cookie** so the flag will continue to be active or inactive for subsequent requests. For all subsequent requests, the cookie value is respected. Waffle only sets cookies if a given flag was actually used during the request.

Flags are global: once a given flag is set for a user, that flag is set on every request or page view. You can test a flag on `/foo` and trust that a given user will see the same flag value on `/bar`.

Flags can be used in **templates**, in **views**, or even to **hide whole views**.

Optionally, Waffle can activate flags based on the **query string**, so you can guarantee a flag will be on or off for any request for testing.

Check out [**Waffle** on Github][1] and let me know what you think!

 [1]: http://github.com/jsocol/django-waffle