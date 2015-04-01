---
title: Using supervisorctl with linux permissions but without root or sudo
author: James
layout: post
permalink: /using-supervisorctl-with-linux-permissions-but-without-root-or-sudo-977/
pw_single_layout:
  - 1
bitly_url:
  - http://j0.is/14e0ktI
bitly_hash:
  - 14e0ktI
bitly_long_url:
  - http://coffeeonthekeyboard.com/using-supervisorctl-with-linux-permissions-but-without-root-or-sudo-977/
categories:
  - Articles
tags:
  - administration
  - devops
  - maintenance
  - server
  - sysop
---
I love [supervisord][1], it&#8217;s been a fantastic way to manage things like [gunicorn][2] and [celery][3] processes. But I didn&#8217;t like that I needed to use `sudo` to restart a running server, e.g.:

    sudo supervisorctl restart todaysmeet-web
    

A quick look through the docs didn&#8217;t reveal how to fix this (it&#8217;s there but not in a task-oriented, easy-to-find way) and a quick search of the web turned up something [close][4] to what I wanted, but not exactly. (If you don&#8217;t care about using normal permissions, that method of using the TCP socket instead of the unix socket works great.)

Here&#8217;s how to do it.

In the `/etc/supervisord.conf` file, probably near the top, you&#8217;ll see a section called [`[unix_http_server]`][5]. Adjust the following settings:

    [unix_http_server]
    file=/var/tmp/supervisord.sock
    chmod=0770
    chown=nobody:web
    

In my case, on all my web servers, the users who have permissions to do things are in the `web` group, so I `chmod=0770` to give the group read/write access to the socket and then `chown=nobody:web` to set the group. You could also set it to a specific user besides `root` or `nobody`, e.g. `chown=james:james` and leave the mode at `0700` to lock it down for one user.

Then you just need to make sure `supervisorctl` is communicating over the unix socket and not the TCP socket. In the [`[supervisorctl]`][6] section, just make sure `serverurl` is set correctly:

    [supervisorctl]
    serverurl=/var/tmp/supervisord.sock
    

Hope that helps someone else!

 [1]: http://supervisord.org/
 [2]: http://gunicorn.org/
 [3]: http://www.celeryproject.org/
 [4]: http://drumcoder.co.uk/blog/2010/nov/24/running-supervisorctl-non-root/
 [5]: http://supervisord.org/configuration.html#unix-http-server-section-settings
 [6]: http://supervisord.org/configuration.html#supervisorctl-section-settings