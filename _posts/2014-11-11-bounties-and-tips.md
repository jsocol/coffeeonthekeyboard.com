---
title: Bounties and Tips
author: James
layout: post
permalink: /bounties-and-tips-1204/
sharing_disabled:
  - 1
pw_single_layout:
  - 1
categories:
  - Articles
tags:
  - Code
  - community
  - open source
  - programming
---
[Last week][1] I wandered into [the tip4commit fracas][2] and a &#8220;helpful&#8221; commenter pointed out Bountysource, so I started asking some questions there. There must have been a few others doing the same thing because, to their credit, they [started a discussion][3] about it.

Since then I&#8217;ve been thinking about money in open source and what exactly it is that makes me uncomfortable with things like tip4commit and, yes, Bountysource. Why am I uncomfortable? Does it extend to other projects like Codesy.io (yes) or Gratipay (no)? What do I think is a better solution?

I have no problem with people being paid to do open source work.

I do have a problem with *maybe* paying people.

I have a problem conflating &#8220;merged&#8221; and &#8220;done.&#8221;

<!--more-->

### Tip4Commit

I don&#8217;t want to talk about Tip4Commit, except to say that they incentivize the wrong thing (lots of commits) and their attitude is [bizarre][4] (who would ever be unhappy about unsolicited email and a quarter of a cent?!).

But, when faced with that, they did add the blacklist. Credit—not, you know, a lot—where it&#8217;s due.

### Libraries

Most of the open source work I&#8217;ve done has been on libraries: they aren&#8217;t end-user applications, but they&#8217;re the tools to build them.

Periodically, someone wants to [add proxy support][5] to django-ratelimit. I always say no, because [proxy configuration is too deployment-specific][6].

But let&#8217;s say, for whatever reason, Alice would rather add proxy support to the ratelimit decorator than use one of the workarounds. Because the code is under an [Apache License][7], she is free to clone the code, make the change, and run her copy. She is, further, free to redistribute the changed version, if she wants. (C.f. [django-brake][8].)

If Alice did that at work, she was paid for her time. If it was for a personal project, she was paid exactly what she is paid for the rest of her personal project work (which might be nothing, of course, but that&#8217;s her call). It doesn&#8217;t matter if I never merge the pull request. In fact, Alice never has to *open* a pull request. **The deliverable was the functionality and it&#8217;s done.**

For library code, &#8220;done&#8221; and &#8220;merged&#8221; are two completely different things. If Bob wants a feature in a library, he can gamble and put a bounty on it, or he can do it himself, or pay Alice to do it for him. If he really believes the library is better with the feature and I&#8217;m not willing to merge it, he&#8217;s free to release the fork.

I&#8217;m not involved in any of those transactions, nor do I want to be. If Alice or Bob decide to contribute the change back in the form of a pull request, then I&#8217;m free to decide whether it belongs in the project regardless of any money changing hands.

Alice did the work, the work was useful—or not—and she was compensated according to fair rules that *have nothing to do with me, the maintainer*.

### Applications

A reasonable way to distinguish an application from a library is that merging *does* matter. There&#8217;s one running copy or one canonical distributor. For example, GitHub just put a [$1000 bounty on an old Firefox bug][9].

In this case, for the work to be &#8220;done,&#8221; it really does need to be in Firefox—and make it all the way to hundreds of millions of computers. Unlike a library, GitHub doesn&#8217;t benefit from running their own Firefox fork.

The first critical piece of that is buy-in from the maintainers. If the Firefox module owners don&#8217;t believe they should do it, no bounty is going to matter.

So let&#8217;s say they do, but, being a small team, they don&#8217;t have the time to do it themselves.

Like Bob, GitHub has a couple of options. They employ hundreds of programmers, maybe someone there has enough background with Firefox to write the patch. Or they could contract someone who has done work on Firefox. Or, they can gamble on it being done and put a bounty on it.

Here, my last issue goes away: **the deliverable is the functionality but it isn&#8217;t delivered until it&#8217;s distributed**. Since distribution requires buy-in from the maintainers and distributors, they should be free to choose whether bounties make sense for their community.

I do, however, still have other issues with bounties.

### The High Bounty Problem

GitHub said this issue is worth $1000 to them. OK. Divide that by your freelance rate, and if you think it&#8217;s worth it, go for it.

But remember to multiply *that* by the odds that you get paid.

If it really is worth no more than $1000 to GitHub, then they admit that they are perfectly happy for this bug to never be fixed, if the market can&#8217;t match them with a developer. If they&#8217;re just bidding low, and are willing to raise the price, maybe they should just hire someone and agree on deliverables.

### The Low Bounty Problem

But, they say, &#8220;[Bounties should always be treated as rewards, and never as guarantees][10]&#8220;. They should be icing on the cake, a thank-you for something you&#8217;d do anyway.

People are not rational when it comes to money. [The Ultimatum Game][11] is a result that&#8217;s surprising to no one (I got to see RadioLab do this experiment live, most people rejected anything less &#8220;fair&#8221; than 60/40).

In the world of open source, I think it goes something like this:

My time writing patches, reviewing pull reqs, corralling issues, etc, is a gift&#x2a;. (If you&#8217;re doing work for hire, it&#8217;s a gift from your employer.) It&#8217;s a gift to the world at large, and it&#8217;s worth whatever my time is worth.

<span style="color:#999">&#x2a; There are, of course, benefits to me for doing it.</span>

Once you offer me money for that time, it&#8217;s no longer a gift, it&#8217;s a transaction. And it&#8217;s worth whatever you offered.

If I thought the gift of my time was worth more than what you offered, I may be insulted, even if the &#8220;rational&#8221; answer is that it&#8217;s free money for something I would have done anyway. I may reject the &#8220;unfair&#8221; deal.

On the other hand, if someone offered me a beer (ahem, [Justin][12]) it&#8217;s a gesture of thanks. Even though it&#8217;s worth nearly nothing—most places, anyway—it&#8217;s, as with any gift, the thought that counts.

Whether the motivation is intrinsic (I want/need this to work) or extrinsic (someone will pay me to), I don&#8217;t buy that bounties are a good primary or extra incentive. If people behaved rationally and all bounties were small enough to be secondary, maybe they would be.

If anyone has data showing meaningful metrics improvements even correlated with bounties, please do let me know.

### Gratipay

The other novel idea in open-source funding is [Gratipay][13], where money is entirely divorced from deliverables.

When talking about compensation, bonuses and commissions are for what you *did*, while salaries and raises are for what we expect you *to do*. Gratipay, interestingly, tries to approach money in open source from the salary side.

(I had a Gratipay account, but card numbers changed and I forgot about it, etc, so I shut it down.)

Because it&#8217;s to a specific person, and completely separate from any specific work, Gratipay feels much more like a thank-you-for-the-work-you-do *gift*. If I look at Gratipay, the problem I see it solving is that I can&#8217;t buy you a beer from across the country, not crowd-funding a development contract.

Is that the real problem? I don&#8217;t know, and so I don&#8217;t know if Gratipay is the right solution. But it&#8217;s certainly worth exploring.

### Addendum: GitHub and Opting In

Finally, because it comes up a lot, just because I put something on GitHub, does that mean I opted-in to Bountysource?

GitHub obviously has [terms of service][14] that I did agree to and the code we&#8217;re talking about is usually open source (though not always: if it doesn&#8217;t have a license it isn&#8217;t FLOSS, even if it&#8217;s on GitHub; and when it is, the license covers use, modification and redistribution of the code, not butting into my community management).

But those are red herrings, because if your claim is that [what you&#8217;re doing is not illegal][15] then maybe you need to step back in to the realm of *should* you do it?

If you&#8217;re going to interact with the management of an open source project in an ongoing way, you should ask the people managing it. If your idea is great (hi, Travis CI!) people will opt-in. If people don&#8217;t opt-in, you need to figure out why.

 [1]: /biweekly-ish-update-07nov2014-1185/
 [2]: https://news.ycombinator.com/item?id=8542969
 [3]: https://github.com/bountysource/frontend/issues/930
 [4]: https://github.com/tip4commit/tip4commit/issues/9
 [5]: https://github.com/jsocol/django-ratelimit/issues/55
 [6]: https://django-ratelimit.readthedocs.org/en/latest/security.html#client-ip-address
 [7]: http://www.apache.org/licenses/LICENSE-2.0
 [8]: https://pypi.python.org/pypi/django-brake
 [9]: https://www.bountysource.com/issues/3949300-add-x-content-type-options-nosniff-support-to-firefox
 [10]: https://github.com/bountysource/frontend/issues/930#issuecomment-62485941
 [11]: http://www.smbc-comics.com/?id=3507#comic
 [12]: https://twitter.com/lintzston
 [13]: https://gratipay.com/
 [14]: https://help.github.com/articles/github-terms-of-service/
 [15]: http://xkcd.com/1357/