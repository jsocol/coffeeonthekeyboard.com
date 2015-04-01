---
title: First Impressions of RockMelt
author: James
layout: post
permalink: /first-impressions-of-rockmelt-498/
bitly_url:
  - http://j0.is/19hLU4v
bitly_hash:
  - 19hLU4v
bitly_long_url:
  - http://coffeeonthekeyboard.com/first-impressions-of-rockmelt-498/
categories:
  - Articles
---
I finally got around to installing the [RockMelt][1] beta, and have had it open for a day or so now, sort of using it in parallel with [Firefox 4][2]. This is a very early impression, and not terribly organized. Once I&#8217;ve used it more I&#8217;ll try to be a little more comprehensive.

*I&#8217;m honestly not trying to come off as a Firefox 4 sycophant, but as it&#8217;s the browser I use for all my non-work browsing, it&#8217;s my natural comparison. All opinions are my own and in no way represent Mozilla.*

**As a browser**

RockMelt is still just a browser at its core. It&#8217;s based on [Chromium][3], and it keeps a lot of stuff going in parallel. Firefox with 14 tabs is using about 450MB of RAM right now, plus around 21MB for Flash, in two processes. RockMelt, with 4 tabs, has 11 processes using 260MB. I know it&#8217;s maintaining connections to Twitter and Facebook, but apparently not very efficiently.

It&#8217;s Chromium; it&#8217;s WebKit; it&#8217;s fine. There&#8217;s nothing to complain about in the rendering or modernism departments. The OmniBar still doesn&#8217;t hold a candle to the AwesomeBar. It adds a Firefox-style dedicated search field, which is strange with the OmniBar focuses so heavily on search results. (Both fields include social results from the attached services, I really don&#8217;t know what the search field is for.)

It imported my bookmarks from Firefox automatically, which is thoughtful, but it really lacks the UI to do much with them. I&#8217;ve seen work from Mozilla&#8217;s UX team that tells me bookmarks can be better: I&#8217;m still waiting for someone to do it.

RockMelt feels like a step backwards from Firefox 4 for day-to-day browsing. I miss the AwesomeBar, App Tabs, and Panorama most. It&#8217;s probably a completely horizontal move from Chrome, though.

**As a social tool**

Obviously the reason to use RockMelt isn&#8217;t it&#8217;s spectacular browsing experience, it&#8217;s the *social* browsing experience. Unfortunately, &#8220;social&#8221; means &#8220;Facebook and Twitter.&#8221;

RockMelt builds in two very popular services but it really falls short with any others. The left side of the browser shows all my Facebook chat contacts—which would be great if I used Facebook chat. The right includes widgets that let me stay up to date with Facebook and Twitter by opening in-app (which I can pull to outside the app) lightweight clients.

There&#8217;s an &#8220;experimental&#8221; Gmail Notifier which works well-enough. There are two Facebook widgets, one for notifications and one for my News Feed. I can add a widget for any RSS/Atom feed, which doesn&#8217;t do much for me personally but I could certainly see having value.

The most active social tool is a &#8220;Share&#8221; button next to the OmniBar, that let&#8217;s me post a link to Twitter or Facebook. [F1][4] adds the same thing to Firefox and includes Gmail, though, admittedly the UI in RockMelt is better (smaller, faster, feels more native).

I understand why Twitter and Facebook were first, but I wouldn&#8217;t have shipped a beta without at least some Gmail and/or Google Talk integration. If I could replace the Facebook Chat border with Google Talk it would be a lot more valuable to me. Social results in the OmniBar should include my Google contacts.

**As a Twitter client**

The built-in Twitter client disappoints me. It updates too rarely and lacks features I&#8217;ve come to expect in a Twitter client, most notably a way to view conversations (it&#8217;s only supported on the timeline tab, not the mentions tab where it would be more valuable). It tries to be helpful by including images and videos inline but I honestly find that more annoying than I think they intended, and I can&#8217;t find a way to turn it off.

Viewing replies is either two clicks, or I have to leave open a column that takes maybe 20-25% of the width of the client for nothing but the tabs. A horizontal tab strip would have used a lot less space.

That leaves me leaving Twitter open in a pinned tab, which I can already do in Firefox 4 or Chrome.

**As a Facebook client**

I probably don&#8217;t use Facebook enough to judge this well. Having two widgets instead of tabs in one widget seems silly. It doesn&#8217;t keep itself very up-to-date, either, and seems slightly less functional than my phone. Again, I think I&#8217;d just leave Facebook open in a pinned tab.

**Ultimately&#8230;**

After using RockMelt for just a day, there are a few things I would like to see (not just in RockMelt).

  * **Better Gmail integration**. An unread message count just doesn&#8217;t do it for me anymore.
  * **Google Talk integration**. I don&#8217;t use Facebook Chat, but do use Google Talk. It would add a lot of value for me.
  * **Google Voice integration**. I don&#8217;t use it (yet) but it&#8217;s potentially a high-value addition.
  * **Atom Publish Protocol support**. Support for APP is disappointingly low, in general, but the option to post to my WordPress blog would be a great &#8220;Share&#8221; option. Also for F1.
  * **Bookmark(let)s on the right side**. Along with the RSS widgets, sticking a &#8220;Share on Tumblr&#8221; or &#8220;bit.ly&#8221; bookmarklet, for example, would open up a lot more services.
  * **Better UI for F1**. I realize Mozilla&#8217;s F1 is a pretty early prototype, but I&#8217;d like to see the UI improve in a few ways: 
      * **Smaller**. F1 is pretty big; I don&#8217;t need that much space for a Tweet.
      * **Less obtrusive**. Rather than opening along the entire top of the page, pushing down the content, something similar to RockMelt&#8217;s &#8220;Share&#8221; button or Firefox 4&#8217;s doorhanger notifications would stay out of the way.
      * **Faster**. I get the impression F1 is loading remote content for its UI and I have no idea why.
  * **Post everywhere**. This applies to RockMelt and F1, but I&#8217;d like to be able to update Facebook and Twitter simultaneously.

I would also totally use a Firefox add-on that gave me a RockMelt-style thin sidebar for Google Talk. [Yoono][5] might be close, I&#8217;ll give it a shot.

The experience of RockMelt is definitely evolutionary rather than revolutionary, at least if you&#8217;re used to interacting with these services regularly. I don&#8217;t feel like it adds significant value over the native services or Firefox with the right add-ons, but I do think it has some good ideas for UX in the space, that should be further developed.

 [1]: http://www.rockmelt.com/
 [2]: http://nightly.mozilla.org/
 [3]: http://www.chromium.org/
 [4]: http://f1.mozillamessaging.com/
 [5]: https://addons.mozilla.org/en-US/firefox/addon/1833/