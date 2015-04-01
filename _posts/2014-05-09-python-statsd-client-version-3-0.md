---
title: Python StatsD Client Version 3.0
author: James
layout: post
permalink: /python-statsd-client-version-3-0-1118/
pw_single_layout:
  - 1
dsq_thread_id:
  - 2672743275
categories:
  - Articles
---
I just pushed version 3.0 of my Python [StatsD client][1] which contains a few doc updates but one significant, backwards-incompatible change: **the default statsd client instance has moved**.

There are now two (and possibly more in the future) places to find a ready-to-go client, so the upgrade should be very easy. Instead of:

    from statsd import statsd
    

You&#8217;ll just need to do one of these:

    from statsd.defaults.django import statsd
    from statsd.defaults.env import statsd
    

Doing this fixed several code smell issues and helps clean things up.

  * There are no unexpected import-time requirements for `import statsd`.
  * Nothing from Django or the environment is imported unless you specifically ask for it.
  * So there are no import errors to catch. (If you try to `from statsd.defaults.django import statsd` and Django settings can&#8217;t be imported, that&#8217;s a legitimate error and we don&#8217;t catch it.)
  * You always *know* where the configuration came from. (If there&#8217;s a Django settings error you won&#8217;t accidentally get an environment-configured instance.)
  * The environment-configured instance is now always available using defaults. No more checking to see if `STATSD_HOST` is defined even if it&#8217;s localhost.
  * All the default configuration values are now available at `statsd.defaults.{HOST,PORT,PREFIX,MAXUDPSIZE}`, which will hopefully be helpful for downstream projects.

No other import paths have changed. For example, Andy&#8217;s [`django-statsd`][2] works great with v3, because it imports `statsd.client.StatsClient`, which is unchanged.

And now adding new default instances in the future scales far more cleanly. If there are more environments that support this kind of global configuration it&#8217;s easy to add new modules to the `statsd.defaults.` namespace.

 [1]: https://pypi.python.org/pypi/statsd/
 [2]: https://github.com/andymckay/django-statsd