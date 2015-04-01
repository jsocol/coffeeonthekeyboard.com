---
title: 'Responsible SQL: How to Authenticate Users'
author: James
layout: post
permalink: /responsible-sql-how-to-authenticate-144/
bitly_url:
  - http://j0.is/14dZxcg
bitly_hash:
  - 14dZxcg
bitly_long_url:
  - http://coffeeonthekeyboard.com/responsible-sql-how-to-authenticate-144/
categories:
  - Database
  - MySQL
  - PHP
tags:
  - attack
  - Back-end
  - Code
  - Database
  - injection
  - MySQL
  - PHP
  - security
  - sql
---
Most SQL-injection articles set a horrible example for young programmers.

Here is a very typical &#8220;bad example&#8221; of why you need to escape user data before it goes into SQL queries:

(ed. The symbol « is a line break that’s not in the real code.)

<div class="dean_ch" style="white-space: wrap;">
  <ol>
    <li class="li1">
      <div class="de1">
        <span class="re0">$username</span> = <span class="re0">$_POST</span><span class="br0">&#91;</span><span class="st0">&#8216;username&#8217;</span><span class="br0">&#93;</span>; <span class="co1">// username=admin</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        <span class="re0">$password</span> = <span class="re0">$_POST</span><span class="br0">&#91;</span><span class="st0">&#8216;password&#8217;</span><span class="br0">&#93;</span>; <span class="co1">// password=&#8217; OR 1=1; &#8212; &#8216;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        <span class="re0">$user</span> = <span class="re0">$db</span>-><span class="me1">query</span><span class="br0">&#40;</span><span class="st0">"SELECT * FROM users WHERE «</span>
      </div>
    </li>
    
    <li class="li2">
      <div class="de2">
        <span class="st0"> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; username=&#8217;$username&#8217; AND «</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        <span class="st0"> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; password=&#8217;$password&#8217; LIMIT 1;"</span><span class="br0">&#41;</span>;
      </div>
    </li>
  </ol>
</div>

The point, of course, is that you must sanitize your user input, or else this person would run this query:

<div class="dean_ch" style="white-space: wrap;">
  <ol>
    <li class="li1">
      <div class="de1">
        <span class="re0">$user</span> = <span class="re0">$db</span>-><span class="me1">query</span><span class="br0">&#40;</span><span class="st0">"SELECT * FROM users WHERE «</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        <span class="st0"> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; username=&#8217;admin&#8217; AND «</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        <span class="st0"> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; password = &#8221; OR 1=1; &#8212; &#8216; LIMIT 1;"</span><span class="br0">&#41;</span>;
      </div>
    </li>
  </ol>
</div>

Which grants the sneaky user all your admin privileges. Other versions have nefarious users dropping your users or articles tables.

The problem is: this is the wrong way to authenticate users. These examples are written for beginners to understand the importance of sanitizing input, but they also provide a model to those beginners for how user authentication works. And it&#8217;s a very bad model.

This is a long one, more after the break.<!--more-->

The only upside to authenticating this way is that you don&#8217;t expose any information on failure, that is, if I&#8217;m trying to hijack someone&#8217;s account, I can&#8217;t tell the difference between an invalid user name and a valid user name with a bad password. That&#8217;s good, but there are good reasons not to do this at the database level.

The &#8220;correct&#8221; way is not much more complex. Basically:

  1. Look up the record with the **username** only.
  2. Get the (hashed) password out of the database.
  3. Hash the submitted password.
  4. Compare the two hashes.

This is really not very hard to implement. In PHP:

<div class="dean_ch" style="white-space: wrap;">
  <ol>
    <li class="li1">
      <div class="de1">
        <span class="coMULTI">/**</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        <span class="coMULTI">&nbsp;* Check a password against the database</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        <span class="coMULTI">&nbsp;*</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        <span class="coMULTI">&nbsp;* @<a href="http://twitter.com/param">param</a> string $username The username to check</span>
      </div>
    </li>
    
    <li class="li2">
      <div class="de2">
        <span class="coMULTI">&nbsp;* @<a href="http://twitter.com/param">param</a> string $password The (supposed) password</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        <span class="coMULTI">&nbsp;* @<a href="http://twitter.com/return">return</a> int 0=success, 1=bad username, 2=bad password</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        <span class="coMULTI">&nbsp;*/</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        <span class="kw2">function</span> check_password <span class="br0">&#40;</span> <span class="re0">$username</span>, <span class="re0">$password</span> <span class="br0">&#41;</span> <span class="br0">&#123;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; <span class="re0">$db</span> = <span class="kw2">new</span> mysqli<span class="br0">&#40;</span><span class="br0">&#41;</span>; <span class="co1">// we need to talk to the DB</span>
      </div>
    </li>
    
    <li class="li2">
      <div class="de2">
        &nbsp;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; <span class="co1">// the real_escape_string() function is much better</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; <span class="co1">// than add_slashes() for escaping MySQL database input</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; <span class="re0">$_username</span> = <span class="re0">$db</span>-><span class="me1">real_escape_string</span><span class="br0">&#40;</span><span class="re0">$username</span><span class="br0">&#41;</span>;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp;
      </div>
    </li>
    
    <li class="li2">
      <div class="de2">
        &nbsp; &nbsp; <span class="co1">// I try to make my SQL queries as easy to read</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; <span class="co1">// as possible. (Not always very easy.)</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; <span class="re0">$result</span> = <span class="re0">$db</span>-><span class="me1">query</span><span class="br0">&#40;</span><span class="st0">"SELECT password "</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; .<span class="st0">"FROM users "</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; .<span class="st0">"WHERE username = &#8216;{$_username}&#8217; "</span>
      </div>
    </li>
    
    <li class="li2">
      <div class="de2">
        &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; .<span class="st0">"LIMIT 1;"</span><span class="br0">&#41;</span>;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; <span class="co1">// we&#8217;re assuming the query ran correctly</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; <span class="co1">// if we can&#8217;t return a row, then there&#8217;s no user with</span>
      </div>
    </li>
    
    <li class="li2">
      <div class="de2">
        &nbsp; &nbsp; <span class="co1">// that name</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; <span class="kw1">if</span> <span class="br0">&#40;</span> !<span class="re0">$user</span> = <span class="re0">$result</span>-><span class="me1">fetch_assoc</span><span class="br0">&#40;</span><span class="br0">&#41;</span> <span class="br0">&#41;</span> <span class="br0">&#123;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw1">return</span> <span class="nu0">1</span>; <span class="co1">// return code for bad username</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; <span class="br0">&#125;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp;
      </div>
    </li>
    
    <li class="li2">
      <div class="de2">
        &nbsp; &nbsp; <span class="co1">// now, assuming the password was hashed with crypt()</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; <span class="kw1">if</span> <span class="br0">&#40;</span> <span class="re0">$user</span><span class="br0">&#91;</span><span class="st0">&#8216;password&#8217;</span><span class="br0">&#93;</span> != «
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <a href="http://www.php.net/crypt"><span class="kw3">crypt</span></a><span class="br0">&#40;</span><span class="re0">$password</span>, <span class="re0">$user</span><span class="br0">&#91;</span><span class="st0">&#8216;password&#8217;</span><span class="br0">&#93;</span><span class="br0">&#41;</span> <span class="br0">&#41;</span> <span class="br0">&#123;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; &nbsp; &nbsp; <span class="kw1">return</span> <span class="nu0">2</span>; <span class="co1">// return code for bad password</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; <span class="br0">&#125;</span>
      </div>
    </li>
    
    <li class="li2">
      <div class="de2">
        &nbsp;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; <span class="kw1">return</span> <span class="nu0"></span>; <span class="co1">// return code for success</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        <span class="br0">&#125;</span>
      </div>
    </li>
  </ol>
</div>

What&#8217;s going on here? Basically, we&#8217;re looking up the user by the username. If we don&#8217;t find a user, we throw out an error. If we do find a user, we re-encrypt the password they supplied, and check it against the encrypted password we already have. If they don&#8217;t match, we throw out an error. If they do, the user is allowed to log in.

There are two key differences between this method and the method so often espoused by tutorial writers:

  1. This method stores an encrypted password instead of plain text.
  2. This method differentiates between bad usernames and bad passwords.

#1 should be obvious. Never store an unencrypted password. It&#8217;s extremely dangerous: if someone ever gets a look at the table, they can just read the users&#8217; passwords—which may well be the same as their bank password (no it shouldn&#8217;t be, but it probably is). And it&#8217;s unnecessary. Every server-side language implements the MD5 hash, which is weak but works. Better options (like PHP&#8217;s <a onclick="window.open(this.href,'newwindow'); return false;" href="http://www.php.net/crypt">crypt()</a>) can use algorithms like Triple-DES, SHA1, Blowfish, or at least MD5 with a random salt.

But wait, #2, I said it was better *not* to distinguish between a bad username and a bad password, right? Well&#8230; yes, to the end user. In either case, I should display a message like &#8220;Bad username or password&#8221; to the person who tried to log in.

Internally, however, I want to know what happened. Is someone targetting known users, or just trying random combinations? How did they find real usernames? Where should I be improving security?

You&#8217;re also minimizing the number of user-submitted strings that get sent to the database. There are fewer opportunities for you to accidently allows an injection attack. If you have a policy on username syntax, you can keep yourself even safer by not talking to the database if the username is bad:

(I&#8217;ve omitted logging or real error-handling here. In a live version, I would probably wrap most of this in a `<a onclick="window.open(this.href,'newwindow'); return false;" href="http://us2.php.net/manual/en/language.exceptions.php">try</a>` block, throw one of three types of exceptions, and do some logging in the `catch` block.)

<div class="dean_ch" style="white-space: wrap;">
  <ol>
    <li class="li1">
      <div class="de1">
        <span class="kw2"><?php</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        <span class="co1">// Usernames must start with a letter, and contain</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        <span class="co1">// only letters, numbers, underscores and dots, but</span>
      </div>
    </li>
    
    <li class="li2">
      <div class="de2">
        <span class="co1">// must not end with a dot or underscore.</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        <span class="re0">$user_regex</span> = <span class="st0">&#8216;/[a-zA-Z][a-zA-Z0-9_<span class="es0">\.</span>]*[a-zA-Z0-9]/&#8217;</span>;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        <span class="kw1">if</span> <span class="br0">&#40;</span> <a href="http://www.php.net/preg_match"><span class="kw3">preg_match</span></a><span class="br0">&#40;</span><span class="re0">$user_regex</span>,<span class="re0">$username</span><span class="br0">&#41;</span> <span class="br0">&#41;</span> <span class="br0">&#123;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; <span class="co1">// the username matches our allowed syntax</span>
      </div>
    </li>
    
    <li class="li2">
      <div class="de2">
        &nbsp; &nbsp; <span class="re0">$auth</span> = check_password<span class="br0">&#40;</span><span class="re0">$username</span>, <span class="re0">$password</span><span class="br0">&#41;</span>;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; <span class="kw1">if</span> <span class="br0">&#40;</span> <span class="re0">$auth</span> === <span class="nu0"></span> <span class="br0">&#41;</span> <span class="br0">&#123;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; &nbsp; <span class="co1">// the do_login() function is an exercise</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; &nbsp; <span class="co1">// to the reader</span>
      </div>
    </li>
    
    <li class="li2">
      <div class="de2">
        &nbsp; &nbsp; &nbsp; do_login<span class="br0">&#40;</span><span class="re0">$username</span><span class="br0">&#41;</span>;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp; &nbsp; <span class="br0">&#125;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        <span class="br0">&#125;</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        <span class="co1">// the username was bad, or the username/password</span>
      </div>
    </li>
    
    <li class="li2">
      <div class="de2">
        <span class="co1">// was wrong</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        <span class="co1">// die() is an overly simplistic choice, here.</span>
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        <a href="http://www.php.net/die"><span class="kw3">die</span></a><span class="br0">&#40;</span><span class="st0">"Bad username or password."</span><span class="br0">&#41;</span>;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        &nbsp;
      </div>
    </li>
    
    <li class="li1">
      <div class="de1">
        <span class="kw2">?></span>
      </div>
    </li>
  </ol>
</div>

Obviously we still escape the username, to make damn sure, but this gives us another place to get information. Did someone actually enter `'; DROP TABLE users; --` into our login form, or did they just mistype their password.

I&#8217;m going to end with a request: if you&#8217;re about to write a tutorial for beginners, please be aware of what you&#8217;re modeling in your examples. If you&#8217;re doing something you would never do, for the sake of simplicity or because it&#8217;s not the focus of the tutorial, point that out. Link to another tutorial or at least mention that it&#8217;s a bad way to do something.

Don&#8217;t send a quiet message that wrong is OK.