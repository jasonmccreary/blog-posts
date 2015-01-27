---
title: 'Be a Better PHP Developer: The Basics'
author: Jason McCreary
excerpt: >
  After reviewing thousands of lines of code over the last few years, I compiled a
  list of simple tips to help you be a better PHP developer.
layout: post
comments: true
permalink: /2012/08/better-php-developer/
categories:
  - Main Thread
tags:
  - development
  - php
description: After reviewing thousands of lines of code over the last few years, I compiled a list of simple tips to help you be a better PHP developer.
---
As a [consultant][1] I see a lot of code. I also dedicate a few hours per week to [StackOverflow][2]. Unfortunately a lot of the PHP code I see is a candidate for [Jeremy Kendall][3]‘s [CSI: PHP][4]. PHP has a bum rap as *unsophisticated*. But this isn&rsquo;t PHP&rsquo;s fault. It&rsquo;s the developers. Better code starts with *you*! Time to step up and be a better PHP developer.

I created an initial set of tips that will help you become a better PHP developer. While these can be abstracted to any language, my examples are specific to PHP. Furthermore, I have left out those I felt were personal preference – such as code formatting or avoiding PHP&rsquo;s alternative syntax.

## Don&rsquo;t use PHP short tags

While I appreciate the shorthand, avoiding three additional keystrokes is the ultimate laziness. Especially when three keystrokes cost more than you realize – namely compatibility and portability.

Yes, [`short_open_tags`][5] is an INI setting. Yes, it was re-enabled by default in PHP 5.3. But not every server runs PHP 5.3. Not everyone can modify their INI. Using XML? How do you differentiate between an XML header?

PHP short tags can get messy and confusing. Use `<?php`. It&rsquo;s clear and says, *&ldquo;Hey bitches, I&rsquo;m writing PHP!&rdquo;*.

## Stop using the MySQL library

Please stop using [`mysql_*` functions][6] as they have not been updated in quite some time and are [in the deprecation process][7]. Use [MySQLi][8] or [PDO][9].

If available, I recommend going straight to PDO. Do not pass MySQLi. Do not collect technical debt. However, read the docs for more details between [choosing the API][10].

## RTM: String and Array

I see a lot of code for parsing or manipulating strings and merging or sorting arrays. Often this code is custom written. If I had a nickel for every line of code to search a string with a regex or sorting a multidimensional array…

PHP has thousands of native functions. Over 100 of these are [array functions][11] and [string functions][12]. Take 15 minutes to click through **each** of these functions and read the function definition. You will find gems like `strpos()`, `parse_str()`, `array_filter()`, and `array_reduce()`.

PHP has excellent documentation. Once you're done with String and Array, you should browse the other [PHP functions][13].

## Go forth and be a better PHP developer

I guarantee putting these tips into practice will put you on the path to becoming a better PHP developer. In the end, it&rsquo;s about knowing your language and staying current. On that note, you should jump over and read my post about [Routines of a Good Developer][14].

*This post is part of a series on [How to be a Better PHP Developer][15].*

 [1]: http://jason.pureconcepts.net/web-iphone-developer-louisville/ "Jason McCreary - Web and iOS Application Developer. Consultant."
 [2]: http://stackoverflow.com/users/164998/jason-mccreary "Jason McCreary on StackOverflow"
 [3]: https://twitter.com/jeremykendall/ "Jeremy Kendall on Twitter"
 [4]: http://csiphp.com/blog/ "CSI: PHP"
 [5]: http://www.php.net/manual/en/ini.core.php#ini.short-open-tag "PHP INI: short_open_tag"
 [6]: http://co.php.net/manual/en/ref.mysql.php "PHP MySQL Functions"
 [7]: http://news.php.net/php.internals/53799
 [8]: http://php.net/manual/en/book.mysqli.php
 [9]: http://php.net/manual/en/book.pdo.php
 [10]: http://php.net/manual/en/mysqlinfo.api.choosing.php
 [11]: http://php.net/array "PHP Array Functions"
 [12]: http://php.net/string "PHP String Functions"
 [13]: http://www.php.net/manual/en/funcref.php "PHP Function Reference"
 [14]: http://jason.pureconcepts.net/2009/12/good_developer_routines/ "Routines of a Good Developer"
 [15]: http://www.google.com/search?q=site%3Ajason.pureconcepts.net&q=%22Be+a+Better+PHP+Developer%22 "How to be a Better PHP Developer"
