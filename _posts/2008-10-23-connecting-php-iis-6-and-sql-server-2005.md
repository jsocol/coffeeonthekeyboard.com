---
title: Connecting PHP, IIS 6, and SQL Server 2005
author: James
layout: post
permalink: /connecting-php-iis-6-and-sql-server-2005-129/
bitly_url:
  - http://j0.is/14dZzRg
bitly_hash:
  - 14dZzRg
bitly_long_url:
  - http://coffeeonthekeyboard.com/connecting-php-iis-6-and-sql-server-2005-129/
categories:
  - Database
tags:
  - Back-end
  - Code
  - Database
  - iis
  - microsoft
  - pdo
  - PHP
  - sql server
---
I know I will be accosted for this, but at work we needed to run PHP on IIS 6 ([fairly simple][1]) and connect it to a remote database server running SQL Server 2005 (not terrible, once I gave up the Microsoft way).

Yeah yeah, do it in ASP.NET, I know. While I like C# as a language, I kind of hate ASP.NET as a framework, so what are you gonna do? Java was an option but the start-up time was too long for this project.

My first Google search for &#8220;PHP SQL Server 2005&#8243; turned up the Microsoft [SQL Server 2005 Driver for PHP][2]. &#8220;Well great!&#8221; I thought. It&#8217;s just a PHP extension, very easy to install on Windows. But I didn&#8217;t know the horrid depths into which I was about to sink.

The Microsoft driver comes with an example application and database. The application assumes you are connecting to a local database. There is scant information about remote databases.

The driver defines this function:

<pre>sqlsrv_connect($host[, $connectionOptions[, ...]]);</pre>

The example application tells you to set `$host` to <var>(local)</var>. Supposedly this works. However, after scouring the internet for several days, and trying every permutation of hostname, Windows networking name, port, IP address, white space, and several other variables that shouldn&#8217;t have been in there, I&#8217;ve decided it doesn&#8217;t talk to remote servers nicely.

[PDO][3]&#8216;s ODBC driver, on the other hand, and a quick visit to [www.connectionstrings.com][4], worked wonderfully.

Here is how I needed to create the PDO object. I hope this is useful for someone else:

(ed. The symbol « is a line break that&#8217;s not in the real code.)

<pre>$host     = '1.2.3.4';
$port     = '1433';
$database = 'MyDatabase';
$user     = 'MyDatabaseUser';
$password = 'MyDatabasePassword';

$dsn = "odbc:DRIVER={SQL Server}; «
 SERVER=$server,$port;DATABASE=$database";

try {
  // connect
  $conn = new PDO($dsn,$user,$password);
} catch (PDOException $e) {
  // fancy error handling
}</pre>

 [1]: http://www.peterguy.com/php/install_IIS6.html
 [2]: http://www.microsoft.com/sqlserver/2005/en/us/PHP-Driver.aspx
 [3]: http://us.php.net/manual/en/book.pdo.php
 [4]: http://www.connectionstrings.com/