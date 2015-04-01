---
title: Weekly Update for 11/3/11
author: James
layout: post
permalink: /weekly-update-for-11311-575/
bitly_url:
  - http://j0.is/14dZME6
bitly_hash:
  - 14dZME6
bitly_long_url:
  - http://coffeeonthekeyboard.com/weekly-update-for-11311-575/
categories:
  - Articles
tags:
  - mozilla
  - node python
  - sumo
  - webdev
  - weekly update
---
Been a busy week!

  * Helped run down an issue with our ads on Reddit.
  * Updated [django-multidb-router][1]. 
      * Learned a little about [ContextDecorator ][2]and how to do that in Python 2.6.
  * Shipped [SUMO 2.6.1][3].
  * Wrapped up [SUMO 2.6.2,][4] ready to go this weekend.
  * Rolling through activity logs in [SUMO 2.7][5]. 
      * Building models.
      * Reviewing code.
  * Did some work on [Rasputin][6]: 
      * Github let me have the URL [github.com/rasputin][7]!
      * [rasputin-node][8] is way more node-like. 
          * I could really use input on the [AMQPBackend][9].
      * Added better threaded and multiprocess dispatchers to [rasputin for Django][10]. 
          * I think I need to punt on the logging module and go with [Logbook][11].
          * Want to get an AMQPBackend started.
  * Updated [logbot][12] to work with node 0.5.0 and [daemonize.node][13].
  * Fixed a [Bleach bug][14].
  * Spent a long time tracking down an issue with celery: it just won&#8217;t quit. Literally, it doesn&#8217;t much care about SIGINT and SIGTERM.

Now to start a busy weekend!

 [1]: https://github.com/jbalogh/django-multidb-router
 [2]: http://docs.python.org/dev/whatsnew/3.2.html#contextlib
 [3]: http://moxie.jamessocol.com/bugstats/sumo/2.6.1
 [4]: http://moxie.jamessocol.com/bugstats/sumo/2.6.2
 [5]: http://moxie.jamessocol.com/bugstats/sumo/2.7
 [6]: http://rasputinproject.org
 [7]: https://github.com/rasputin
 [8]: https://github.com/rasputin/rasputin-node
 [9]: https://github.com/rasputin/rasputin-node/tree/amqp
 [10]: https://github.com/rasputin/rasputin
 [11]: https://github.com/mitsuhiko/logbook
 [12]: https://github.com/jsocol/logbot
 [13]: https://github.com/indexzero/daemon.node
 [14]: https://github.com/jsocol/bleach/commit/d9f2cf6cc