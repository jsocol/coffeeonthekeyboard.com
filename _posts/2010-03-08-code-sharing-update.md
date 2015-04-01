---
title: Code-sharing Update
author: James
layout: post
permalink: /code-sharing-update-361/
bitly_url:
  - http://j0.is/16FvqPS
bitly_hash:
  - 16FvqPS
bitly_long_url:
  - http://coffeeonthekeyboard.com/code-sharing-update-361/
categories:
  - Articles
tags:
  - amo
  - Code
  - django
  - mozilla
  - python
  - sumo
  - team
---
When we decided to [move SUMO to a new platform][1], one of the reasons we chose [Django][2] was code sharing and reuse—specifically that [SUMO][3] and [AMO][4] would be able to share code, meaning both teams would save time and see benefits.

So how is that going? Were we right in our assumption here? The code we&#8217;re sharing so far:

[MultiDB Router][5]
:   A Django DB router that supports reading from a pool of slave databases.

[Cache Machine][6]
:   A powerful caching library for Django that, in particular, provides automatic object caching and invalidation through the ORM.

[Jingo][7]
:   An adapter for using [Jinja2][8] templates with Django.

[Django-Nose][9]
:   A test runner for Django using [Nose][10].

[Django Debug Cache Panel][11]
:   Adds a cache panel for [Django Debug Toolbar][12].

[Test-Utils][13]
:   Tools we use testing in the Django/Jinja2/Nose setup.

[Bleach][14]
:   A library for sanitizing and linkifying user HTML, based on [html5lib][15].

[Fixture Magic][16]
:   Django management commands for working with fixture data.

Additionally, we expect both teams will probably use the following, eventually:

[DidYouMean][17]
:   A wrapper for [Hunspell][18], using [PyHunspell][19] to provide spelling suggestions for searches.

[Django Gearman][20]
:   Provides an easier interface from Django to the Python [Gearman][21] bindings.

AMO&#8217;s JS and CSS minification
:   AMO has already solved the problem of JS and CSS minification with Django and Jinja2.

And it&#8217;s not a released library, but SUMO has also been able to directly reuse code from AMO to simplify pagination.

Overall, it seems like we&#8217;re doing really well on this! It&#8217;s great to see the projects not just sharing code, but packaging and publishing it on Github and [PyPI][22]. If any of the above is useful to you, go ahead and try it out! You can open issues with any of the packages on Github, or find us in [#webdev][23] in irc.mozilla.org.

 [1]: http://coffeeonthekeyboard.com/the-evolution-of-sumo-339/
 [2]: http://www.djangoproject.com/
 [3]: http://support.mozilla.com/
 [4]: https://addons.mozilla.org
 [5]: http://github.com/jbalogh/django-multidb-router
 [6]: http://github.com/jbalogh/django-cache-machine
 [7]: http://github.com/jbalogh/jingo
 [8]: http://jinja.pocoo.org/2/
 [9]: http://github.com/jbalogh/django-nose
 [10]: http://somethingaboutorange.com/mrl/projects/nose/0.11.2/
 [11]: http://github.com/jbalogh/django-debug-cache-panel
 [12]: http://github.com/robhudson/django-debug-toolbar
 [13]: http://github.com/jbalogh/test-utils
 [14]: http://github.com/jsocol/bleach
 [15]: http://code.google.com/p/html5lib/
 [16]: http://github.com/davedash/django-fixture-magic
 [17]: http://github.com/jsocol/didyoumean
 [18]: http://hunspell.sourceforge.net/
 [19]: http://code.google.com/p/pyhunspell/
 [20]: http://github.com/fwenzel/django-gearman
 [21]: http://gearman.org/
 [22]: http://pypi.python.org/pypi
 [23]: irc://irc.mozilla.org/webdev