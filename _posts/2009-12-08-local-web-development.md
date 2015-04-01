---
title: Local Web Development
author: James
layout: post
permalink: /local-web-development-323/
bitly_url:
  - http://j0.is/6jrz14
bitly_hash:
  - 6jrz14
bitly_long_url:
  - http://coffeeonthekeyboard.com/local-web-development-323/
categories:
  - Articles
tags:
  - linux
  - mozilla
  - vm
  - web development
  - webdev
  - windows
---
I&#8217;m not ashamed of it: I like Windows. I think the user experience is light-years ahead of Gnome and KDE. There&#8217;s nothing ostensibly *wrong* with OS X, but there are little usability differences and frankly switching isn&#8217;t worth the annoyance to me. That&#8217;s why I run Windows 7 on all three computers I use daily.

This is only a problem when I try to work on LAMP web applications. Sure, I could install [XAMPP][1], but running Apache/PHP on Windows is really not close enough to a production environment. So I have two choices: I can dual-boot Linux and work in an OS—well, a window manager—I don&#8217;t like, or I can turn to virtual machines.<!--more-->

[VMWare][2] is a pretty heavyweight solution. [Fusion][3], for OS X, is a great product. But in Windows, I opt for Sun&#8217;s [VirtualBox][4].

<div id="attachment_324" style="width: 310px" class="wp-caption aligncenter">
  <img class="size-medium wp-image-324" title="virtualbox-new" src="http://coffeeonthekeyboard.com/wp-content/uploads/2009/12/virtualbox-new-300x206.png" alt="VirtualBox New Virtual Machine Wizard" width="300" height="206" />
  
  <p class="wp-caption-text">
    VirtualBox New Virtual Machine Wizard
  </p>
</div>

VirtualBox lets me run Linux (I opt for [Fedora][5] as it&#8217;s close to our production environment and I like it) in just another window, right next to Firefox.

To create a new VM, you&#8217;ll only need VirtualBox and a Linux ISO. (Or, you could use a pre-existing VM, or an &#8220;appliance,&#8221; copied from somewhere else. I&#8217;m not going to cover that.) Step through the wizard. Some of my recommended settings:

<div id="attachment_325" style="width: 310px" class="wp-caption aligncenter">
  <img class="size-medium wp-image-325" title="virtualbox-disk" src="http://coffeeonthekeyboard.com/wp-content/uploads/2009/12/virtualbox-disk-300x268.png" alt="VirtualBox New Disk Settings" width="300" height="268" />
  
  <p class="wp-caption-text">
    VirtualBox New Disk Settings
  </p>
</div>

  * At least 512 MB of RAM for running Gnome+Apache+MySQL+VIm(+Firef0x?) in the VM.
  * At least 30 GB of hard disk. Yes, you can add more later, but expanding an existing disk is difficult, and if you use a &#8220;Dynamically expanding storage&#8221;-type of disk (see above) it won&#8217;t take the full 30 GB right off the bat.
  * There are two ways to go about doing Network settings—I do both. 
      * Use Bridged networking. This gives your VM an IP address accessible to the rest of your network.
      * Use NAT. This gives your VM an IP address on a virtual network that only exists in your computer, but with a virtual gateway giving your VM access to the internet.
      * (You can also use Host-only networking, but that would prevent your VM from accessing the internet at all, and that&#8217;s no good.)
  * Mount your installation media ISO as a CD/DVD drive via VirtualBox. You&#8217;ll be able to boot off the ISO and install Linux. You may run into issues if you leave the ISO mounted after installation.

<div id="attachment_326" style="width: 310px" class="wp-caption aligncenter">
  <img class="size-medium wp-image-326" title="virtualbox-network" src="http://coffeeonthekeyboard.com/wp-content/uploads/2009/12/virtualbox-network-300x260.png" alt="VirtualBox Network Settings" width="300" height="260" />
  
  <p class="wp-caption-text">
    VirtualBox Network Settings
  </p>
</div>

Once you&#8217;re in Linux, set up your web server environment (use yum, apt-get, or whatever other package manager you choose). The key software I use:

  * Apache
  * PHP
  * MySQL
  * Memcached
  * Sphinx
  * Squid

With the software above, I can very nearly replicate the production environment for our web applications, while still spending most of my time in Windows. (When I do switch to the VM, I&#8217;m usually in VIm, my favorite text editor, anyway.)

Apache/PHP/MySQL is the typical PHP app stack. We use memcached for output caching (on [SUMO][6], anyway). We use Sphinx to power our search engine. Squid is useful from time to time for approximating an application behind a reverse-proxy cache and load-balancing server. (You could also use nginx or Varnish.)

If you use NAT or Bridged networking, you&#8217;ll be able to navigate to your VM&#8217;s IP address from a browser on the host computer (in my case, in Windows). By using Bridged, I can access my local Apache instance from browsers on Windows, Linux, or a nearby Mac, which is immensely valuable when doing browser support work and testing. I can even send my VM&#8217;s IP address to other people in the office and have them look at my local work.

There is a performance cost with a VM. HTTP requests would come back faster in a real Linux environment, and sometimes I do boot into Linux, but the convenience is worth it to me.

 [1]: http://www.apachefriends.org/en/xampp.html
 [2]: http://www.vmware.com/
 [3]: http://www.vmware.com/products/fusion/
 [4]: http://www.virtualbox.org/
 [5]: http://fedoraproject.org/get-fedora
 [6]: http://support.mozilla.com/