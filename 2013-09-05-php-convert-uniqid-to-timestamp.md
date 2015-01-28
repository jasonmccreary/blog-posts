---
layout: post
title: "Convert uniqid() to a timestamp"
heading: "Convert uniqid() to a timestamp"
date: 2013-09-05 15:04
comments: true
categories: 
- Main Thread
excerpt: "A proof on how you can convert <code>uniqid()</code> to a Unix timestamp in PHP."
subheading: "A proof on how you can convert <code>uniqid()</code> to a Unix timestamp in PHP."
description: A proof on how you can convert uniqid() to a Unix timestamp in PHP.
---
I came across an interesting [question on StackOverflow](http://stackoverflow.com/questions/18642234/convert-uniqid-to-timestamp "Convert uniqid() to timestamp"). Unfortunately the question was closed before I could answer. I'd like to answer it.

> Can I convert `uniqid()` to a timestamp?

*Sort of*.

From the [PHP documentation on `uniqid()`](http://php.net/manual/en/function.uniqid.php):

> without being passed any additional parameters the return value is little different from `microtime()`

The comments note that `uniqid()` outputs a hexadecimal string. So let’s convert `microtime()` to a hexadecimal string and compare it to `uniqid()`.

``` php
	<?php
	$microtime = microtime(true);
	$id = uniqid();

	echo dechex($microtime);
	echo PHP_EOL;
	echo $id;
```

``` text Script Output
	5228cee5
	5228cee5564a0
```

We see both share the same prefix (`5228cee5`). So what are the remaining characters of `uniqid()`?

Turns out the answer is pretty obvious. It's the microseconds. But `uniqid()` does not simply multiply `$microtome` by 1,000,000. Instead it appends the microseconds as a hexidecimal string.

Let’s take another look:

``` php
	<?php
	$microtime = microtime();
	$id = uniqid();

	list($microseconds, $timestamp) = explode(' ', $microtime);
	$suffix = str_replace(dechex($timestamp), '', $id);

	echo $microseconds, PHP_EOL;
	echo '0.', hexdec($suffix), PHP_EOL;
```

``` text Script Output
	0.23929900
	0.239327
```

Pretty close. The few nanosecond difference is the runtime between executing line 1 and 2.

So using `microtime()` we've proven `uniqid()`, without parameters, is the concatenation of a timestamp and microseconds as hexadecimal strings.

Why then did I say *sort of*?

The suffix. If you run the last script enough you'll notice an inconsistency for low microsecond values.

``` text Script Output
	0.00997400
	0.9984
```

Notice the leading zeroes are missing. So you can’t get a timestamp *with microsecond precision* from `uniqid()`.

However, given this inconsistency, can we trust the suffix is a specific number of hexadecimal characters (i.e. 5)?

The documentation states, without parameters, `uniqid()` returns 13 characters. That said, the simplest code to get the timestamp from `uniqid()` is to extract the prefix:

``` php
	<?php
	$timestamp = substr(uniqid(), 0, -5);
	echo date('r', hexdec($timestamp));
```

``` text Script Output
	Thu, 05 Sep 2013 15:55:04 -0400
```

Why the negative anchor? Consider the Unix timestamp `4294967296`. You don't want to start *Y2.1K*!

**After this exercise** I reviewed the [source code for `uniqid()`](https://github.com/php/php-src/blob/master/ext/standard/uniqid.c "uniqid() source code") to confirm using a negative anchor (`-5`) is indeed safe.

So *yes*, you can convert `uniqid()` to a timestamp (without microsecond precision).
