---
title: Help Me Scale
author: James
layout: post
permalink: /help-me-scale-97/
bitly_url:
  - http://j0.is/14dZxce
bitly_hash:
  - 14dZxce
bitly_long_url:
  - http://coffeeonthekeyboard.com/help-me-scale-97/
categories:
  - Database
  - MySQL
  - PHP
tags:
  - Back-end
  - Code
  - Database
  - db
  - load
  - microblog
  - MySQL
  - PHP
  - Projects
  - query
  - social messaging
  - subquery
  - twitter
---
I&#8217;ve been reading Eran Hammer-Lahav&#8217;s [intelligent][1] [posts on][2] [microblog scalability][3], and now I&#8217;m concerned about my own &#8220;microblog&#8221; site, [Picofiction][4].

Similar to social networks, social updates, social messaging, social&#8230; Like many social web sites—amongst our weaponry&#8230;—Picofiction lets you &#8220;follow&#8221; your favorite authors, displaying all their posts along with yours.

I handle this very naïvely: everything is offloaded to the database. There are three tables involved here, one of users, one of posts, and one of follower/followee bindings.

Here&#8217;s the basic structure of this query:

<pre>SELECT post_id, post_body, post_date, post_type,
  user_name AS author_name, user_id AS author_id
FROM posts
LEFT JOIN users
ON posts.author_id = users.user_id
WHERE author_id = '<var>CURRENT_USER</var>'
OR author_id IN (
  (SELECT followed_id
   FROM followers
   WHERE following_id = '<var>CURRENT_USER</var>')
  )
ORDER BY post_date DESC
LIMIT <var>PAGE_START</var>,20;</pre>

Here&#8217;s where I need help: this works great on a single database, but it does not scale horizontally.

Since this horizontal scalability is such a hot topic right now, I&#8217;m asking for ideas. I&#8217;d like to put in the infrastructure *before* there is a need for it.

Eran points out that caching is not as simple a solution as we&#8217;d like to think. What do you cache? How do you keep caches in sync?

Does anyone have experience with MySQL Cluster Servers? It seems like the best way of scaling is to make the process as [parallelizable][5] as possible. The database then handles the parallelization, so the less I can do in the program the better, right?

 [1]: http://www.hueniverse.com/hueniverse/2008/04/scaling-a-micro.html
 [2]: http://www.hueniverse.com/hueniverse/2008/03/scaling-a-micro.html
 [3]: http://www.hueniverse.com/hueniverse/2008/03/on-scaling-a-mi.html
 [4]: http://picofiction.com/
 [5]: http://en.wikipedia.org/wiki/Amdahl%27s_law