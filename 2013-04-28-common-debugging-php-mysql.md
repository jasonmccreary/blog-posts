---
layout: post
title: "Common debugging for PHP and MySQL"
heading: "Common debugging for PHP and MySQL"
date: 2013-04-28 22:28
comments: true
categories: 
- Main Thread
excerpt: "This post provides a basic checklist for common database debugging when developing with examples in PHP and MySQL."
subheading: "This post provides a basic checklist for common database debugging when developing with examples in PHP and MySQL."
description: "This post provides a basic checklist for common database debugging when developing with examples in PHP and MySQL."
subheading: "This post provides a basic checklist for common database debugging when developing with examples in PHP and MySQL."
---
I see [many questions](http://stackoverflow.com/search?q=%5Bphp%5D+%5Bmysql%5D+%22database+error%22) on StackOverflow about database issues. This post aims to provide a checklist to help diagnose common issues. While this post contains PHP and MySQL code samples, this debugging checklist applies to other database platforms.

First, [debugging is hard](http://blog.jvroom.com/2012/02/08/debugging-hard-problems/ "Debugging hard problems"). Especially debugging database issues. Often times the best approach is to systematically rule out what cannot be the problem. This checklist adopts such an approach from a low to high level.

## Can you connect to the database outside your application?
Verify you can connect to your database by logging into MySQL from the command line.

	mysql -u dbuser -p -h localhost database

If you have a specific database user for your application, be sure to verify their credentials as well.

If you do not have command line access, you can use another database administration tool (e.g. PHPMyAdmin).

If you cannot connect to the database, you need to start at the beginning: Ensure MySQL is running, your database exists, and your credentials are correct.

## Can you connect to the database inside your application?
Verify you can connect to the database from PHP. Test with a separate script to also rule out bugs in your codebase:

``` php
<?php
$link = mysqli_connect('localhost', 'my_user', 'my_password', 'my_db');

if (!$link) {
    die('Connect Error (' . mysqli_connect_errno() . ') '
            . mysqli_connect_error());
}

echo 'Connected... ' . mysqli_get_host_info($link) . "\n";
```

If this code connects, but your application code does not, debug your application code.

If this code does not connect use the output for clues. You can also check your PHP error logs. It's likely your MySQL module is misconfigured. Use `phpinfo()` to review your MySQL configuration.

## Does your query run successfully?
More often than not the query is the problem. Especially if the query is generated dynamically. The best way to verify your query it to output and run it yourself.

``` php
$sql = 'SELECT column FROM table WHERE column = $bad_var';
echo $sql;
if (!$mysqli->query($sql)) {
    echo 'Error: ', $mysqli->error;
}
```

In this case, we'd see that `$bad_var` is not set. As such, the query becomes:

	SELECT column FROM table WHERE column = 

**Note:** This code above is a contrived example of a dynamic query. If you do not see *what else* is wrong with this query, please read about [SQL injection](http://stackoverflow.com/questions/60174/how-to-prevent-sql-injection-in-php/60496 "How to prevent SQL injection in PHP").

You can debug in any order. Top down or bottom up. Just remember this is by no means exhaustive. Nonetheless, following this debugging checklist will help diagnose a *majority* of your database issues. Please share other common database debugging you use.

