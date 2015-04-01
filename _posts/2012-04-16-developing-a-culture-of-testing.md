---
title: Developing a Culture of Testing
author: James
layout: post
permalink: /developing-a-culture-of-testing-642/
bitly_url:
  - http://j0.is/138JpPj
bitly_hash:
  - 138JpPj
bitly_long_url:
  - http://coffeeonthekeyboard.com/developing-a-culture-of-testing-642/
dsq_thread_id:
  - 1746519708
categories:
  - Articles
---
I say this all the time, but Mozilla&#8217;s webdev group has [grown a lot][1] over the past few years, and I don&#8217;t just mean in size. We&#8217;ve become better engineers, a better team, too.

One key aspect of that growth, and a dramatic shift from three years ago, is developing a **Culture of Testing**.

It&#8217;s nearly impossible to find a developer who will flat-out disagree with the statement, &#8220;Testing is good.&#8221; But it&#8217;s easy to find developers who don&#8217;t write tests. There are lots of ways to rationalize not writing tests, like&#8230;

  * We have tests but they&#8217;re slow and old and not worth updating.
  * We&#8217;re trying to move quickly here!
  * Management won&#8217;t give me the time.
  * QA should write tests.
  * And many others.

Back around when I started, we were guilty of this. When projects had test suites at all, they were too slow to run frequently, often filled with broken or out of date tests, and generally not very valuable. Most projects had some form of automated [Selenium][2] test suite, but that was about it. Nothing in the project code itself.

Now all major projects, especially new ones, have automated test suites. We look at something like 90-95% line coverage in those suites. How did we make that change?

We took a few specific steps. We didn&#8217;t necessarily plan these, but in retrospect they are pretty clear, repeatable actions that other teams should be able to adapt and use.

  * We started with the premise that **tests are good**. This isn&#8217;t always trivial, and you may need to convince people. *Tests are an investment,* because they do take more time up front than not testing, but *they will pay off in the long run* in faster development and fewer regressions.
  * We **required tests for new code**. We were in a fortunate position, beginning to move two web apps to a new language and framework, but with a little investment in bootstrapping the test suite, this should work anywhere. The lead developers of those two apps simply required tests as part of code reviews. It&#8217;s important to be consistent and firm, especially at the beginning, to establish expectations.
  * We **set up [Jenkins][3]**. Any CI service will work. It doesn&#8217;t need to be perfect, either. At first, we had Jenkins polling our Git repo for changes every few minutes. Later, we got it to run tests on push instead of pull. The important things were that *we were running tests soon after commits* and *results were reported in IRC*. Getting feedback immediately is critical.
  * And we set up the **[CI Game plugin][4]**. It doesn&#8217;t matter so much now, but gamifying tests helped motivate people to write them and fix them (and fix code standard violations) early on.
  * We **made broken tests priority zero**. If someone pushes code and breaks tests, they are expected to drop everything else and fix the tests. This is a big part of *keeping master/trunk shippable*. If you [deploy continuously][5], it&#8217;s even more critical.
  * We **tease people who break tests**. Not cruel mockery, but light social pressure is a powerful tool. Peer pressure: it works!

Over time, the team internalized these expectations, shifting our development culture in real ways.

  * **A feature isn&#8217;t done unless it&#8217;s tested**, so the time to write the tests is not counted separately from the time to write any other code.
  * **Everyone is responsible for tests**, not just QA. We&#8217;ve freed up a bunch of our web QA engineers&#8217; time to do more exploratory testing of new features and improve both automation tools and old, crufty test suites.
  * **Tests are first class code**, and improving methodology or performance in tests is as valid a use of time as anywhere else.

Because these changes have happened across the entire group, new team members are surrounded by this culture, adopt the practices, and internalize it very quickly.

This was a very successful shift, and has helped the team grow in a number of ways. The specific actions might not work for every team, but they worked for us and are definitely worth trying, if you&#8217;ve been struggling&mdash;or are just starting&mdash;to adopt great testing practices.

 [1]: http://blog.mozilla.com/webdev/2011/08/08/pragmatic-growth-from-2-to-40-in-4-years/
 [2]: http://seleniumhq.org/
 [3]: https://ci.mozilla.org/
 [4]: https://wiki.jenkins-ci.org/display/JENKINS/The+Continuous+Integration+Game+plugin
 [5]: http://blog.mozilla.org/sumo/2012/03/29/sumo-deploying-continuously/