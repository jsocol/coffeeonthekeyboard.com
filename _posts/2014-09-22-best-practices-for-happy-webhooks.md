---
title: Best Practices for Happy Webhooks
author: James
layout: post
permalink: /best-practices-for-happy-webhooks-1138/
pw_single_layout:
  - 1
dsq_thread_id:
  - 3043336455
categories:
  - Articles
---
I love webhooks. I love automating things and minimizing the [shit work][1] I and my users have to do, and webhooks have been an invaluable solution to a real problem.

I will also say right up front that I haven&#8217;t implemented outbound webhooks all the way into production. I&#8217;ve [started to][2] a few times and never released it. But I&#8217;ve received more than a few webhooks, and these are the things that, as a receiver, make me happy.

## HMAC-based Authentication

I need to know that I can trust the source of the webhook. IP addresses can be spoofed, and DNS can be compromised, but fortunately we have cryptographic tools! And even better, HMAC is a very simple tool to use.

[Mandrill][3] sends an HMAC header, as does [GitHub][4]. GitHub&#8217;s is slightly easier to calculate.

[Stripe][5] has an annoying answer: they send the full event data and then suggest you pull it back via their API. Given that actually doing that is optional, I&#8217;d rather they either only sent the ID, making it non-optional, or included an HMAC.

Checking the HMAC is also optional, of course, but it&#8217;s optional and faster.

[MailChimp][6] has basically no support for securing webhook data. You can&#8217;t even request the event data from the API to verify it. Their solution: security by obscurity. (Given that it&#8217;s the same folks as Mandrill, this really surprises and disappoints me.)

## Unique Event IDs

Every webhook that gets fired should contain a unique ID *for that event*. Even with HMAC-based auth, an attacker can still record the full request and replay it against application. This is fairly benign in some cases (e.g. a MailChimp `unsubscribe` event) but can be extremely dangerous when using something like Stripe.

Fortunately, Stripe events all have a unique ID, and they do recommend logging which events you&#8217;ve processed. GitHub has `X-Github-Delivery` but I&#8217;m not sure if it&#8217;s specific to the *event* or the *attempt* to deliver it. (I&#8217;ll update this post if they [answer me][7].)

You **absolutely cannot rely on the timestamp**. At any meaningful scale, it&#8217;s entirely possible for two events to have the same timestamp, especially if it&#8217;s only second-resolution. A Mandrill [`click`][8] event, for example, could have the same set of (event type, timestamp, message-ID) if a user clicked twice on a link, and that doesn&#8217;t even require a large set of event sources.

Generate a UUID or however else your system creates unique IDs. It&#8217;s worth it.

## One Event at a Time

For origin servers, it can be tempting to batch several events ([ala Mandrill][9]) to reduce their own load, but this just creates complexity for receivers. Handling one webhook per request makes keeping track of replay (see above) and retry (see below) much simpler and thus more reliable.

## Retry with Back-off

This is probably the most basic of all webhook requirements: if the destination server isn&#8217;t available, the origin should retry it at increasing intervals. How long the retries should continue is up to what&#8217;s reasonable for that use case. Stripe needs to retry harder than Mailchimp, for example.

A good solution is &#8220;any 2xx status code means it was handled, anything else means retry&#8221;. That means receivers can opt to drop certain events on the floor when they&#8217;re working properly.

## Data Format

Don&#8217;t bother [wrapping JSON-encoded data in `multipart/form-data`][9]. Stick to one or the other.

Yeah, this is a small complaint, but it&#8217;s just more pleasant to treat the whole request body the same way.

 [1]: http://zachholman.com/posts/shit-work/
 [2]: http://bundlescout.tumblr.com/
 [3]: http://help.mandrill.com/entries/23704122-Authenticating-webhook-requests
 [4]: https://developer.github.com/webhooks/#delivery-headers
 [5]: https://stripe.com/docs/webhooks#receiving-a-webhook
 [6]: http://apidocs.mailchimp.com/webhooks/#securing-webhooks
 [7]: https://twitter.com/jamessocol/status/514103599983767552
 [8]: http://help.mandrill.com/entries/58303976-Message-Event-Webhook-format
 [9]: http://help.mandrill.com/entries/24466132-Webhook-Format