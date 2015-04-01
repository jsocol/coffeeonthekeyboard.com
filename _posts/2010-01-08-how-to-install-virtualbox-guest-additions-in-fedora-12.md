---
title: How to Install VirtualBox Guest Additions in Fedora 12
author: James
layout: post
permalink: /how-to-install-virtualbox-guest-additions-in-fedora-12-332/
bitly_url:
  - http://j0.is/5NaMJ5
bitly_hash:
  - 5NaMJ5
bitly_long_url:
  - http://coffeeonthekeyboard.com/how-to-install-virtualbox-guest-additions-in-fedora-12-332/
dsq_thread_id:
  - 1746518816
categories:
  - Articles
tags:
  - fedora
  - kernel
  - linux
  - vbox
  - vm
---
**Update:** Whoa! Looks like my instructions only work for 64-bit guests. Scroll down to the bottom for the changes you need to make for a 32-bit Fedora guest.

This was not quite as straightforward as I remember it being in Fedora 11. I ran into a problem and couldn&#8217;t find the solution in 5 minutes of searching, so I offer it here: the steps to install VBox Guest Additions in Fedora 12.

I&#8217;m assuming you&#8217;re using VBox 3.1.2 (latest as of writing) and the kernel version is 2.6.31.9 on an x86_64. What does all that mean? In the long strings of numbers, some of them might change for you.

First, in the VM menu (not the Guest but the chrome around it) go to **Devices > Install Guest Additions**. It will mount a new disc image. Then fire up terminal.

    $ su
    # yum install kernel-headers kernel-devel gcc
    # export KERN_DIR=/usr/src/kernels/2.6.31.9-174.fc12.x86_64
    # cd /media/VBOXADDITIONS_3.1.2_56127
    # ./VBoxLinuxAdditions-amd64.run

This time the kernel modules should compile. Then restart the system.

### Caveats!

There&#8217;s a pretty good chance that some directories will be different, so rather than typing this out, you probably want to make liberal use of the <kbd>tab</kbd> key. Navigate to the right source directory (probably the only one in `/usr/src/kernels`) and then try: `` # export KERN_DIR=`pwd` ``

There&#8217;s also a small chance you might get an error that says `gksu: not found`, and most probably the autorun script won&#8217;t do anything. I ran `# ln -s /usr/bin/sudo /bin/gksu` and it seemed to clear the problem up. (I only ran into this when starting the install via `autorun.sh`, not with the `.run` file.

### Update for 32-bit Guests

A few possible changes if this doesn&#8217;t work for you with a 32-bit guest. (It didn&#8217;t for me, so I had to play around/research a bit more.)

  1. Run `# uname -r`. If you see the letters `PAE`, then you&#8217;ll need to follow the rest of these steps. If you *don&#8217;t* see `PAE`, you should be fine.
  2. If so, make sure your kernel is up to date with `# yum update kernel-PAE`. After this, restart.
  3. Instead of the `kernel-devel` package, you&#8217;ll need to install `kernel-PAE-devel`. That makes the second line of the example above:  
    `# yum install kernel-headers kernel-PAE-devel gcc`
  4. If you&#8217;d already installed the `kernel-devel` package, you may want to remove it: `# yum remove kernel-devel` as it can confuse things.
  5. Then, everything else should be the same.