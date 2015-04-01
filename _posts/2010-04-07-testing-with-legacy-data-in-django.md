---
title: Testing with Legacy Data in Django
author: James
layout: post
permalink: /testing-with-legacy-data-in-django-391/
bitly_url:
  - http://j0.is/bqIACu
bitly_hash:
  - bqIACu
bitly_long_url:
  - http://coffeeonthekeyboard.com/testing-with-legacy-data-in-django-391/
categories:
  - Articles
---
As we incrementally [move SUMO][1] over to [Django][2], we often run into problems with legacy data—I&#8217;d say all of our problems so far, in fact. This one took several hours to figure out, so I thought I&#8217;d share.

Our basic problem is that we want to test our search facility, which is built on [Sphinx][3], including indexing, searching directly, and searching through the web interface. Fortunately, [Dave Dash][4] on the [AMO][5] team tackled this in general, and I was largely able to follow his lead. But then I ran into our legacy data issues.<!--more-->

*Before I even start describing my solution, I want to point out that, yes, I tried basically everything else. I did not go so far as changing the schema of our legacy tables to make this test pass, but I was right up at the brink. I **do not recommend** following this example unless you&#8217;ve exhausted your options.  
*

In order to run our search indexer, and get accurate results, we needed a number of tables to join the the documents we&#8217;re indexing (wiki pages, in this case) to their category information.

My first attempt at making these pass was to use the [initial-SQL][6] method in Django. The downside was that the tests would only pass consistently if they were run with `FORCE_DB`. I was willing to accept that, but then I started reading more about the initial-SQL facility, and realized this would get run on any `syncdb` operation, which was unacceptable.

Then Dave suggested I just create some legacy models so I could build fixtures for them. Well, it was more complex than setting `FORCE_DB=1`, but I gave it a shot.

It turns out that our old CMS, TikiWiki, has a schema so idiosyncratic that it largely *cannot be represented* by models in Django.

First, I couldn&#8217;t find any way to represent a relationship between two of the tables. This part is TikiWiki&#8217;s fault: it joins them on a unique string column, rather than something nice, like an integer. I could deal with that, though, as I realized I didn&#8217;t care if I couldn&#8217;t join them in Django, as long as Sphinx could in pure SQL.

But then, I needed to create a model for a join table so I could use it for `through`, because the old schema used completely wrong names for the foreign key columns, and I hit my first major stumbling point with Django: [every model needs a primary key][7] and [it doesn&#8217;t support multi-column primary keys][8]. I would have needed to alter the table schema to make this work, jeopardizing compatibility with the legacy code.

Lesson: Django will create join tables for you. Don&#8217;t argue with it.

So what to do?

I figured that I could probably just execute raw SQL statements in the setup and teardown methods of my test. So I took the queries out of the initial-SQL files and moved them into setup, and added some `DROP TABLE` statements to the teardown method.

Then I hit a final snag: `CREATE TABLE IF NOT EXIST` issues a [&#8220;Note&#8221;-level warning][9] if the table *does* exist. Django (or [Nose][10]?) considers *any* SQL warning an error, and it seems there&#8217;s no way to prevent that, not even for a single test case.

Fortunately, you can tell MySQL not to issue notes. With a couple of extra queries, I suppressed them and made my tests pass consistently. Thanks to [wdoekes][11] for sharing his solution!

<div class="dean_ch" style="white-space: wrap;">
  <ol>
    <li class="li1">
      <div class="de1">
        <span class="kw1">from</span> django.<span class="me1">db</span> <span class="kw1">import</span> connection
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        cursor = connection.<span class="me1">cursor</span><span class="br0">&#40;</span><span class="br0">&#41;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp;
      </div>
    </li>
    
    <li class="li2">
      <div class="de2">
        cursor.<span class="me1">execute</span><span class="br0">&#40;</span><span class="st0">&#8216;SET @<a href="http://twitter.com/OLD_SQL_NOTES">OLD_SQL_NOTES</a>=@@SQL_NOTES, SQL_NOTES=0;&#8217;</span><span class="br0">&#41;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        <span class="co1"># &#8230; statements that can issues notes &#8230;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        cursor.<span class="me1">execute</span><span class="br0">&#40;</span><span class="st0">&#8216;SET SQL_NOTES=@OLD_SQL_NOTES;&#8217;</span><span class="br0">&#41;</span>
      </div>
    </li>
  </ol>
</div>

 [1]: http://coffeeonthekeyboard.com/the-evolution-of-sumo-339/
 [2]: http://www.djangoproject.com/
 [3]: http://www.sphinxsearch.com/
 [4]: http://spindrop.us/
 [5]: https://addons.mozilla.org/
 [6]: http://docs.djangoproject.com/en/dev/howto/initial-data/#providing-initial-sql-data
 [7]: http://docs.djangoproject.com/en/dev/ref/models/fields/#django.db.models.Field.primary_key
 [8]: http://code.djangoproject.com/wiki/MultipleColumnPrimaryKeys
 [9]: http://bugs.mysql.com/bug.php?id=2839
 [10]: http://code.google.com/p/python-nose/
 [11]: http://code.djangoproject.com/ticket/12293