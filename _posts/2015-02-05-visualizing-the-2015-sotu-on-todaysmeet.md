---
title: Visualizing the 2015 SotU on TodaysMeet
author: James
layout: post
permalink: /visualizing-the-2015-sotu-on-todaysmeet-1246/
pw_single_layout:
  - 1
dsq_thread_id:
  - 3489098189
categories:
  - Articles
tags:
  - Back-end
  - d3
  - distributed
  - django
  - dq
  - javascript
  - leveldb
  - node.js
  - python
  - queues
  - streams
  - todaysmeet
---
*Now that you know [how TodaysMeet works][1], here&#8217;s part 2: using the message queue architecture to build the SotU visualizations.*

[TodaysMeet][2] has a [long history][3] with [political events][4]. During the 2012 Presidential debates, I was at a stranger&#8217;s apartment—guest of a guest—half-listening (binders full of *what did he say*?) and using a hastily borrowed laptop to try to [keep it running][5]. That was the event that convinced me I needed a new platform.

After the 2014 State of the Union, I’d had an idea to do a running word cloud synced to the video: look at what people in “sotu” rooms were saying, and create a new cloud every minute or so. I never got around to it, and after a while it didn’t feel very topical.

But on Sunday, January 18th, two days before the 2015 State of the Union, when I realized it was coming up, I decided to [do it live][6].

<a href="http://knowyourmeme.com/memes/bill-oreilly-rant" target="_blank"><img src="http://i1.kym-cdn.com/entries/icons/original/000/000/792/do-it-live.jpg" width="477" height="361" alt="we'll do it live!" class="aligncenter" /></a>

To do something real-time with the SotU data, I would need to dip into the comment stream and get the text of the messages in rooms related to the speech. Then I’d need to do some analysis on that text and eventually push the data to clients, which would render some sort of visualization.

This ended up as 3 or 5 parts, depending how you count.

<img src="http://coffeeonthekeyboard.com/wp-content/uploads/2015/02/How-SOTU-Works.png" alt="How SOTU Works" width="596" height="436" class="aligncenter size-full wp-image-1250" />

A quick review from [yesterday&#8217;s post][1]:

  * We&#8217;ll call the Django app `tm` for now. It is responsible for web, API, authentication, and talking to the database. (This is the old monolith I&#8217;m [slowly breaking up][7].)
  * `ekg` is a websocket service. Data flows in via the API and out via websockets. It&#8217;s JavaScript on NodeJS and uses [Primus][8].
  * `reflektor` uses an in-process queue to proxy and replay HTTP messages from `tm` to *n* `ekg` processes. 

### flatdb

[flatdb][9] is a simple, [Flask][10]-based HTTP interface to [LevelDB][11].

I built it as part of a PoC a while ago, and kept the code around. LevelDB has the ability to get a range of lexically sorted keys, so it&#8217;s a great way to keep data by timestamp (at least until September 33,658).

Selfishly, I put flatdb on PyPI just so it was easier for my deploy infrastructure. I don&#8217;t know if I&#8217;ll have a good reason to use it again, but maybe it&#8217;ll be useful for someone. After all, I still get pull reqs to bizarre old PHP libraries I threw on GitHub.

There were two instances of flatdb running, one that I called `clouddb` which stored JSON blobs of word frequency data, and one called `tickdb` which included per-second counts.

### sotu-collector

The original plan was to [generate word clouds][12]—hence `clouddb`—every few seconds. I picked 10 second buckets as a balance of frequent and interesting. (Spoiler: the word cloud visualization didn’t work out.)

`sotu-collector` exposes a subset of the same internal API as `ekg`: it pays attention to new comments and changes in room data that cause it to flush the in-process cache. As far as `reflektor` is concerned, it is just another target.

Lots of things go from the API through `reflektor` to `ekg`: new comments, deletes, room topics, state changes like pause or close. `ekg` cares about all of them, but `sotu-collector` does not. The queue task does not consider 404 responses a failure, so it&#8217;s possible to implement only part of the API.

`sotu-collector` handles almost all the heavy lifting (it could have been broken up into 3 or more services with different scaling and CPU requirements). It

  * decides whether the comment is to a SotU-related room (users could opt any room in or out, but rooms with “sotu” or “stateoftheunion” in the name defaulted to “in”, so look up the room name and the opt-in state),
  * splits comments up into words,
  * runs them through a stemmer, a stopword list, and a profanity list,
  * builds a map of word &#x2192; frequency,
  * serializes the map to JSON and flushes it to `clouddb` every 10 seconds,
  * counts the number of relevant messages,
  * runs a quick AFINN-111 [sentiment analyzer][13] on each message,
  * flushes the number of messages and average sentiment (`sum(sentiment)/num_messages`) to `tickdb` every second.

That’s all together because of the time pressure. If it were a more permanent fixture, multiple copies of a collector service could filter relevant messages before sending them to one or more dedicated analysis services.

### sotu-pulse

The other half of the server-side component was pushing the data out to clients. Since TodaysMeet already uses Primus to manage websocket connections, it was a natural choice.

`sotu-pulse` is a simpler service than `sotu-collector`: it maintains streaming connections, and some generic stats (e.g. current connected), periodically pulls new data from `clouddb` (one blob every 10 seconds) and `tickdb` (five ticks every 5 seconds), and pushes that data to the clients.

When it became clear I wouldn’t be able to get the word cloud to work in time, I decided to do a simple bar graph of the relative frequencies of the top 10 words, so the only clean-up `sotu-pulse` had to do was sorting the words by frequency and picking the top 10.

#### The client

The other moving part was the actual visualization.

[<img src="http://coffeeonthekeyboard.com/wp-content/uploads/2015/02/small-sotu.png" alt="the sotu page" width="500" height="271" class="aligncenter size-full wp-image-1253" />][14]

I wanted a place people could watch the speech, participate in their own discussions (especially if a teacher had set up a room for their class) and see the overall trends across all the rooms.

The White House has made huge strides in making the speech accessible: they streamed it live on YouTube. That made watching it easy (and of course you could pause the stream if it was on your TV).

TodaysMeet already supports [embedding a room][15], but I needed to write a small embeddable page to help users join their *own* rooms. (I’m going to use what I built here to make it easier to join rooms from the home page or mobile devices, so if nothing else, this whole thing worth while for that!)

I built the frame for all of this very quickly, to get anything at all on the web to start promoting it. TodaysMeet technically supports IE8—or [did, at the time][16]—but by skipping it for the SotU pages, I could use `calc()` and that was honestly the most helpful thing I could’ve done for myself.

I hadn’t directly used [d3][17] until approximately 10 hours before the SotU, so what I could do was pretty limited. I tried to make the word cloud work for a while, then gave up and decided that I could figure out a bar graph—but couldn’t manage animating the bars in time—and a couple of line graphs (that’s when I decided I had time to add frequency and sentiment graphs).

Fortunately there are some great examples of building [bar][18] and line graphs, even [smoothly scrolling line graphs][19], so I was just able to pull that off under the wire.

### How it went

Really well! My biggest concerns were that common words would be profanity—TodaysMeet has a lot of teenage users—and that it would fall over. In that order.

I opened up the “official” room and the viewer about an hour before the speech started. At first, there plenty of moments when the lines were flat and the most common word was “hey”.

But once the speech started, the common words were on topic, the post volume kept moving, and the sentiment graph was interesting! Either people were behaving alright or the profanity filter worked well enough—it’s one of many things I would’ve instrumented if I’d had more time.

And the SotU processes kept up without too much trouble. With Primus, I’ve noticed that most relevant load numbers seem to scale linearly with the number of connections, but with a nice low slope. `sotu-collector` was handling the same volume of messages as `ekg`, and doing a comparable amount of work per message—though more of it was loop-blocking, CPU-bound text processing.

### Lessons

I’m so happy I decided to do this: it was a ton of fun, and I took away a few things.

  * Going through the process of adding a few new services was valuable—I haven’t done it since August, so it was a nice reminder and sanity check.
  * So was provisioning a new box, that’s not something I do all that often these days.
  * I haven’t actually asked reflektor to double up production messages before, so this was the first real validation that it works as designed and in tests.
  * `calc()` works, unprefixed, in all modern browsers including IE9. This is going to be incredibly helpful (I can’t rely on flexbox yet, but `calc()` is going to solve some very real layout problems especially on embedded and mobile rooms).
  * Working through the UI to join a room by name, instead of by URL, helped me understand the parts I’ll need. Not being able to do this is probably the biggest single problem people run into, and limits how useful TodaysMeet is on mobile devices.
  * If you want a line graph that shows deviation from an axis that isn’t on the edge: draw the axis.
  * None of the graphs had scales, and that was fine. This wasn’t hard science, but it was interesting to see what got people talking, about what, and roughly how they felt.
  * Sentiment analysis is hard. The readily available tool was a bag-of-words analysis, and I really didn’t trust the scores for any of the individual messages I ran through it. But in aggregate it seems to have done a reasonably good job.

Designing and building a product in two days is exhilarating and challenging. When you start any part you have no idea how long it will take. I found it very helpful to have a vision of where I wanted to go that was a little bit blurry. Video, conversation, analysis. What analysis? Well, I’ll see what I can do in time.

### Go services!

This deserves its own post, but&#8230; Service-oriented systems are amazing, and using message queues to push data around asynchronously means it’s very easy to dip into a stream of data. It’s much easier to shut off a service when it’s isolated and not built in to other systems, which makes prototyping—or short-lived projects very cheap.

Pushing data in only one direction helps reason about it. It&#8217;s the door you need to walk through to get to a queue-based architecture.

It&#8217;s critical to understand what guarantees your product actually *needs*. We tend to assume that what we&#8217;re doing is absolutely, life-and-death critical and any delay or disruption is completely unacceptable—because to *us*, it is. But our users may be perfectly happy with half a second delay, if they even notice it! (When you post a comment on TodaysMeet, the UI responds immediately, and backtracks if it runs into an error later. So even if a post took over a second, odds are the user wouldn&#8217;t know.)

The difference between 5 and 500 milliseconds is an entire universe, especially for 90-99%iles. So is the difference between &#8220;it&#8217;s bad&#8221; and &#8220;someone will die&#8221; if a message is dropped. Relax, be honest, and embrace asynchronicity.

 [1]: http://coffeeonthekeyboard.com/how-todaysmeet-works-1237/
 [2]: https://todaysmeet.com
 [3]: http://blog.todaysmeet.com/case-study-meeting-during-debates-88/
 [4]: http://blog.todaysmeet.com/2014-state-of-the-union-room-205/
 [5]: http://blog.todaysmeet.com/presidential-debate-post-mortem-62/
 [6]: http://blog.todaysmeet.com/sotu2015-on-todaysmeet-426/
 [7]: https://github.com/jsocol/talks/tree/master/queensjs-soa
 [8]: http://primus.io
 [9]: https://pypi.python.org/pypi/flatdb
 [10]: http://flask.pocoo.org/
 [11]: https://github.com/google/leveldb
 [12]: http://www.jasondavies.com/wordcloud/
 [13]: https://www.npmjs.com/package/sentiment
 [14]: http://coffeeonthekeyboard.com/wp-content/uploads/2015/02/sotu.png
 [15]: http://blog.todaysmeet.com/embed-todaysmeet-rooms-into-your-class-websites-and-lmses-309/
 [16]: http://blog.todaysmeet.com/browser-support-update-no-longer-supporting-internet-explorer-8-440/
 [17]: http://d3js.org/
 [18]: http://bost.ocks.org/mike/bar/
 [19]: http://bost.ocks.org/mike/path/