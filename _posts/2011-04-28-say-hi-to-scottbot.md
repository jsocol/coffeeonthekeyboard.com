---
title: Say hi to Scottbot
author: James
layout: post
permalink: /say-hi-to-scottbot-594/
bitly_url:
  - http://j0.is/14e5Ooj
bitly_hash:
  - 14e5Ooj
bitly_long_url:
  - http://coffeeonthekeyboard.com/say-hi-to-scottbot-594/
dsq_thread_id:
  - 
categories:
  - Articles
---
**UPDATE:** Scottbot has been removed from GitHub and will not be coming back. [Find out why][1].

After talking about it with [Fred][2] for a couple of weeks, I sat down this morning and started scottbot, an IRC bot that will learn how to make good &#8220;that&#8217;s what she said&#8221; jokes.

It&#8217;s built in [Node.js][3] on two libraries by the inimitable [Harth Vader][4], more commonly known as [Heather Arthur][5]: [nomnom][6], an excellent argparse/optparse implementation for node, and [brain][7], a machine learning library with neural net and Bayesian classifier support. So thanks, Heather!

I&#8217;m also using a slightly hacked [node-irc][8] from Martyn Smith.

Right now it&#8217;s a pretty simple Bayesian method. It&#8217;s been learning all day on [Moznet][9] and has made one or two pretty-good jokes, and a couple of stinkers. I think it&#8217;s already better than our other TWSS bot.

There&#8217;s plenty of things to do to improve it. I don&#8217;t know a whole lot about machine learning or anything Bayesian beyond what I can recollect from my old stats-for-math-majors class, so if you&#8217;ve got any suggestions, I&#8217;d love to hear them (especially in the form of pull requests)! I want to learn, too.

If you&#8217;ve got node and [Redis][10], you, too, can have your very own scottbot.

Scottbot, and it&#8217;s default IRC nick, &#8220;mscott,&#8221; is named affectionately after [Michael Scott][11].

 [1]: http://coffeeonthekeyboard.com/thats-what-he-is-sorry-for-651/
 [2]: http://fredericiana.com/
 [3]: http://nodejs.org
 [4]: http://twitter.com/harthvader
 [5]: https://github.com/harthur/
 [6]: https://github.com/harthur/nomnomargs
 [7]: https://github.com/harthur/brain
 [8]: https://github.com/martynsmith/node-irc
 [9]: http://irc.mozilla.org/
 [10]: http://redis.io/
 [11]: http://www.youtube.com/watch?v=SAAi_42uIkQ