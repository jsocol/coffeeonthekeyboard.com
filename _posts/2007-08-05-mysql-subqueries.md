---
title: MySQL Subqueries
author: James
layout: post
permalink: /mysql-subqueries-48/
blogger_blog:
  - urbaneexistence.blogspot.com
blogger_author:
  - James
blogger_permalink:
  - /2007/08/mysql-subqueries.php
bitly_url:
  - http://j0.is/14dZxsy
bitly_hash:
  - 14dZxsy
bitly_long_url:
  - http://coffeeonthekeyboard.com/mysql-subqueries-48/
categories:
  - Database
  - MySQL
tags:
  - Code
  - Database
  - How To
  - MySQL
  - subquery
---
I often find it difficult to find tips and advice for doing relatively simple things in things like MySQL, Ruby, Python, etc. So, starting with this post, I will help fill that niche. Today&#8217;s topic is **Using Subqueries to Simplify your SQL Queries.**

<p class="note" style="font-style: italic; font-size: 90%; color: #999999">
  For this article, I&#8217;m using PHP and MySQL for examples. There are slightly different implementations of SQL in the various database engines, but this is one thing they all have in common.
</p>

SQL is called &#8220;**s**tructured **q**uery **l**anguage&#8221; because it allows subqueries to make complex queries easier and faster. The idea of a subquery is simple: have the database perform one query and insert it into another.

There are dozens of useful ways of using subqueries, but I will concentrate on two: subqueries in the *select expression* and subqueries in the *where clause*.

### Security Concerns

In most web programming languages, the interface between the script and the database only allows one query per access for security reasons: an injection attack could input something like `'; DELETE * FROM users;` and do some serious damage to a website. Imagine your SQL query to login looked something like:

> `SELECT * FROM users WHERE user_name = '$username' AND password = '$password';`

If you are not checking and cleaning the input appropriately, someone could type the snippet above into your login form and, if multiple queries were allowed, MySQL would execute the following:

> `SELECT * FROM users WHERE user_name = ''; DELETE * FROM users; AND password='';`

Since the empty string wouldn&#8217;t match any rows (hopefully), the first query would be discarded. The second query, the `DELETE` statement, would run, terminating at the second semicolon. Since the third piece of code is nonsense, MySQL would throw it out with an error.

To solve this problem, languages like PHP cause MySQL to issue an error any time there is more text (except comments) after the line terminator, usually the semicolon. The downside is that situations arise where you need to run multiple queries. The result is either often either a godawfully complicated statement with multiple `JOIN`s, or running several queries, each of which requires communication with your database server and can slow down your applications.

In the examples below, I&#8217;ll pretend we&#8217;re building a forum that has four tables:

  * `users` with primary key `user_id`
  * `forums`, a list of all the boards, with primary key `forum_id`
  * `threads` which links each thread to a forum with `forum_id` and has primary key `thread_id`
  * `posts` which links each post to a thread with `thread_id` and has primary key `post_id`

### Subqueries in Select Expressions

One way to speed up your queries again is to use subqueries. Subqueries are full SQL queries nested within another query. For example:

> `SELECT (SELECT * FROM t1);`

Obviously it&#8217;s a pretty simple example. Notice the parentheses. Subqueries must always be in parentheses, even if they are inside a function, like:

> `SELECT MAX((SELECT salary FROM employees));`

Let&#8217;s get to work on our forum. Say that while reading all the threads of a forum you&#8217;d like to have both the number of threads and the number of posts in the forum. One way is to run two separate queries:

> `SELECT COUNT(*) AS threads FROM threads WHERE forum_id='1';<br />
SELECT COUNT(*) AS posts FROM posts LEFT JOIN threads USING(thread_id) WHERE forum_id='1';`

That might not be so bad if your SQL server is `localhost`, but more and more hosts are running dedicated SQL servers, meaning that every query has to run across the internet, be processed, and run back, slowing down your application. But we can run this in one query with two subqueries:

> `SELECT<br />
(SELECT COUNT(*) FROM threads WHERE forum_id='1') AS threads,<br />
(SELECT COUNT(*) FROM posts LEFT JOIN threads USING(thread_id) WHERE forum_id='1') AS posts;`

We can add the above to our query to get the name of the forum and its description, so we can further decrease the number of trips to the database:

> `SELECT<br />
(SELECT COUNT(*) FROM threads WHERE threads.forum_id=forums.forum_id) AS threads,<br />
(SELECT COUNT(*) FROM posts LEFT JOIN threads USING(thread_id) WHERE threads.forum_id=forums.forum_id) AS posts,<br />
forum_name,<br />
forum_description<br />
FROM forums WHERE forum_id='1';`

Notice that we also changed the `WHERE` clauses to match whatever forum ID we put into the &#8220;*outer query*&#8220;.

### Subqueries in Where Clauses

Another simple and useful way to use a subquery is in a `WHERE` clause. Here you must be careful to match the `WHERE` syntax and the type of data returned by the subquery. For example, in `WHERE user_name = (...)`, the subquery (`(...)`) must return a single value, while in `WHERE post_date IN (...)`, the subquery can return a list.

In our forum, we might want to search for all posts by a specific user, but we don&#8217;t want our visitors to need to know the user ID—or perhaps we want a more descriptive URL, like `search.php?user=USER_NAME` instead of `search.php?user=#ID#`. But in our forum, to be efficient, we link posts to their author by the `user_id` column.

One way to do this is to run a query to find the ID then run another query to find the posts. Another way in this particular case is to use a `JOIN` statement. But yet another way is to do this:

> `SELECT * FROM posts WHERE user_id = (SELECT user_id FROM users WHERE user_name = 'foo');`

In the case above, a `JOIN` would also get us the information we want, but in some cases this isn&#8217;t true, for example:

> `SELECT column1 FROM t1<br />
WHERE column1 = (SELECT MAX(column2) FROM t2);`

When you need to `COUNT` or otherwise aggregate one column, you&#8217;ll need to use a subquery instead of a `JOIN`, as well.

### Summary

This article only scratched the surface of subqueries. Subqueries can be nested, they can appear in other places and do other things, and they can make your SQL more readable, among others. I don&#8217;t claim that the SQL statements above are the world&#8217;s most efficient or best way to do things—if you know a better way, let me know! I just want to give an introduction to subqueries, a very basic part of SQL that few people I&#8217;ve met seem to understand.