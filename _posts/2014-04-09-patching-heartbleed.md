---
title: Patching Heartbleed
author: James
layout: post
permalink: /patching-heartbleed-1106/
pw_single_layout:
  - 1
dsq_thread_id:
  - 2598680133
categories:
  - Articles
---
Because this took several hours of trial and error, I want to run through what I did to patch [OpenSSL][1] for [Heartbleed][2] ([CVE-2014-0160][3]) on [TodaysMeet][4]&mdash;and ask for a little help. I hope it saves some folks some time.

The upside of using system package repos is not having to build them yourself. The downside is you take what they give you. I&#8217;ve been compiling RPMs of [nginx][5] for a while, but using the provided OpenSSL RPMs.

The officially patched OpenSSL release is 1.0.1g. The Fedora project&#8217;s most recent build was 1.0.1e. Rather than force people to upgrade to 1.0.1g, [they issued a patched 1.0.1e][6], build 37.

After checking the patched build number I immediately updated. (This was one mistake, I should have been more diligent about checking the current versions first.) I restart nginx, then used [the excellent tester][7] by Filippo Valsorda, but was still showing up as vulnerable.

I got to a dev VM, updated OpenSSL to 1.0.1e-37, and built a new nginx RPM, but after deploying the change and even restarting the server, the tester (and the [SSLLabs test][8]) still detected the vulnerability.

Even after updating OpenSSL via yum, running `openssl version -a` returned something like this:

    OpenSSL 1.0.1e-fips 11 Feb 2013
    built on: Wed Jan  8 07:20:55 UTC 2014
    

Well, that can&#8217;t be good.

But what is nginx actually using?

    $ ldd `which nginx`
    ...
    libssl.so.10 => /lib64/libssl.so.10
    ...
    

OK, so it&#8217;s not using a statically linked version, it should pick up the system changes.

    $ ls -lha /lib64/libssl.so*
    ...
    lrwxrwxrwx 1 root root   16 Apr  9 00:35 /lib64/libssl.so.10 -> libssl.so.1.0.1e
    -rwxr-xr-x 1 root root 429K Apr  8 07:23 /lib64/libssl.so.1.0.1e
    

So that&#8217;s the version, what does it say?

    $ strings /lib64/libssl.so.10 | grep OpenSSL
    ...
    OpenSSL 1.0.1e-fips 11 Feb 2013
    

**Here&#8217;s my first question.** I&#8217;m not sure if that&#8217;s the best way to figure out which version of a linked library is there. Some people were saying that the OpenSSL version line, `OpenSSL 1.0.1e-fips 11 Feb 2013` wouldn&#8217;t change, but the built-on date should be April 7th.

Obviously that wasn&#8217;t cutting it, so I started building an OpenSSL 1.0.1g RPM for Fedora 19.

Many [trials and tribulations][9]&mdash;including hacking up the included `openssl.spec` file and finding a git changeset with an obsolete perl file I needed to include&mdash;later, I had an RPM. I installed it on the web server and restarted nginx.

Still vulnerable.

nginx was still linking to `/lib64/libssl.so.10 => libssl.so.1.0.1e`, the vulnerable version. But the RPM had installed 1.0.1g as `/lib64/libssl.so.1.0.0` and symlinked `/lib64/libssl.so` to it. (**Next question:** why the version mismatch in the file name? Is that OK? Something I can fix in the RPM?)

Then, with the new 1.0.1g RPM installed in the dev VM, I re-built the nginx VM, installed it, and checked that it linked against the correct libssl:

    $ ldd `which nginx`
    ...
    libssl.so.1.0.0 => /lib64/libssl.so.1.0.0
    ...
    

And

    $ strings /lib64/libssl.so.1.0.0 | grep OpenSSL
    ...
    OpenSSL 1.0.1g 7 Apr 2014
    $ openssl version -a
    OpenSSL 1.0.1g 7 Apr 2014
    built on: Tue Apr  8 22:23:48 EDT 2014
    

After installing the latest nginx RPM, both the [Heartbleed test][10] and the [Qualsys test][11] came back clean.

**My last question is**: given that the new libssl file was already on the system, could I have changed which version nginx was linking to without recompiling it? Would changing the symlink been enough, or broken everything?

I hope this was helpful for someone else and that I can learn from it and respond faster next time.

This got really long, but I wanted to rant about [responsible disclosure][12] and how little people seem to understand it. That will have to wait for tomorrow.

 [1]: https://www.openssl.org/
 [2]: http://heartbleed.com
 [3]: http://web.nvd.nist.gov/view/vuln/detail?vulnId=CVE-2014-0160
 [4]: https://todaysmeet.com
 [5]: http://nginx.org
 [6]: https://lists.fedoraproject.org/pipermail/announce/2014-April/003205.html
 [7]: http://filippo.io/Heartbleed/
 [8]: https://www.ssllabs.com/ssltest/
 [9]: https://twitter.com/maxtaco/status/453726047327236096
 [10]: http://filippo.io/Heartbleed/#todaysmeet.com
 [11]: https://www.ssllabs.com/ssltest/analyze.html?d=todaysmeet.com
 [12]: http://blog.cloudflare.com/staying-ahead-of-openssl-vulnerabilities