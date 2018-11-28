---
title: "A Response to &#8220;Micro-Optimizations in PHP&#8221;"
heading: "A Response to &#8220;Micro-Optimizations in PHP&#8221;"
author: Jason McCreary
excerpt: "A response to a post on PHP Micro-Optimizations floating around the PHP community recently."
subheading: "A response to a post on PHP Micro-Optimizations floating around the PHP community recently."
layout: post
comments: true
permalink: /2012/12/php-micro-optimizations/
categories:
  - Main Thread
tags:
  - development
  - php
description: A response to a post on PHP Micro-Optimizations floating around the PHP community recently.
---
Last week I read a post recommended in [PHP Weekly][1] entitled – [Micro-Optimizing in PHP][2]. Always striving to [be a better PHP developer][3], I read this first. The post opens with common recommendations. Most I consider *micro* micro optimizations. As I continued reading some were not optimizations at all. In the end, the post was written without care – making absolute statements without support. 

On a personal note, it bothers me to find such poor quality posts in a PHP newsletter. Posts like this confuse developers and in turn contribute to the poor code notorious in the PHP community. Adding fuel to the fire of the anti-PHP community for [why PHP sucks][4].

I have no doubt the author had good intention. But at the least, he should have provided supporting evidence. After all, a good developer asks *Why*? I reviewed the recommended *optimizations* in the original post. Evaluating recommendations to be *absolute truths* and marking each *true* or *false*. Please read the full section before you segfault.

## `foreach` vs. `for`

**False.**

`foreach` is faster when looping over an array for *reading*. If you need to *write* to the array the performance of `foreach` degrades significantly. So `for` has its place.

Another case for using `for` is when the block contains counting logic.

Consider:

    for ($i = 0; …; ++$i) {
        // block
    }
    

Versus the `foreach` equivalent:

    $i = 0;
    foreach (…) {
        // block
        ++$i;
    }
    

*Exception:* When looping over a sequential numerically indexed array, the array key could serve as a counter.

### `for` Truth

Two absolute truths when using a `for` loop.

1.  **Pre-calculate parts of the condition expression.** The condition within `for` evaluations the condition on each iteration. Avoiding the overhead function calls which return value will not change during the loop will optimize your code.
2.  **Use pre-increment.** [By nature][5], pre-increment is a faster expression.

Code:

    $count = count($arr);
    for ($i = 0; $i < $count; ++$i) {
        // block
    }
    

## Double vs. Single Quotes

**False.**

Back in 2007 I emailed Ilia Alshanetsky about this very matter. He called it an [Optimization Myth][6]. However, that was PHP 4. Somewhere in PHP 5 double quotes were optimized (I *heard* PHP 5.1).

Double quotes performing better than single quotes is counter intuitive. Without the need for variable expansion, it stands to reason single quotes would be faster. Furthermore literal values (single quote) could be optimized in *memory*.

I've benchmarked double quotes versus single quotes several times without finding anything conclusive. Maybe double quotes are indeed faster. But to *replace all single quotes with double quotes* as the original post suggests is not worth the time. In the end, following your [code style][7] is more important.

## `UNION` vs. `OR`

**False.**

I am currently reading [High Performance MySQL][8]. In fact, I just finished the chapter on query optimization. This recommendation actually put me on the path to writing this response post. Stating change all \`OR\` to \`UNION\` is just plain reckless.

First, the original example is bad. It suggests changing:

    select username from users where company = ‘bbc' or company = ‘itv';
    

to:

    select username from users where company = ‘bbd'
    union
    select username from users where company = ‘itv';
    

When using the same column, the opposite of what the author suggests is more performant. That is you should change from a `UNION` to an `OR` when the `WHERE` clause operates on the same column.

Second, changing `OR` to `UNION` in your queries may not return the same result. While `UNION` may be an optimization, you need to understand when to use it. **Do not** sweepingly replace `OR` with `UNION` in you codebase.

As I am admittedly still learning MySQL Optimizations, I posted the topic to the StackOverflow community. I encourage you to [read the answers][9] for more details on why `UNION` vs `OR` is not **always** an optimization.

## Additional PHP Optimizations

The original post did contain a few true optimizations.

*   **Use `echo` versus `print`.** [True][10].
*   **Use string functions over regular expressions.** True, with the exception of complex pattern matching. The equivalent regular expressions code written using string functions will be more performant. This [is noted][11] in the PHP documentation for the regular expression functions.

## The Performance 80/20 Rule

Performance follows the [80/20 Rule][12]. If *single quotes vs double quotes* accounts for 80% – you're optimized. Congratulations. You can go home. That is an absolute truth.

## Remember

There is no silver bullet. Be skeptic of absolute statements. There are many moving parts in a system. What works for someone else may not work for you. When it doubt, [benchmark it yourself][13].

## References

If you are interested in PHP performance and optimizations, check out the following resources:

*   [The PHP Benchmark][14]
*   [PHP Performance Metrics][15]

Also, consult the [PHP Documentation][16]. The function definitions and user comments contain valuable information.

 [1]: http://phpweekly.info
 [2]: http://www.developerknowhow.com/micro-optimizing-in-php/
 [3]: http://jason.pureconcepts.net/2012/08/better-php-developer/
 [4]: http://webonastick.com/php.html
 [5]: http://stackoverflow.com/a/9205011/164998
 [6]: http://www.ilia.ws/files/zend_performance.pdf
 [7]: http://jason.pureconcepts.net/2012/09/code-style-fashion/ "Code: Style vs. Fashion"
 [8]: http://shop.oreilly.com/product/0636920022343.do
 [9]: http://stackoverflow.com/questions/13750475/sql-performance-union-vs-or
 [10]: http://stackoverflow.com/questions/7094118/reference-comparing-phps-print-and-echo
 [11]: http://php.net/manual/en/function.preg-match.php
 [12]: http://www.entrepreneurs-journey.com/397/80-20-rule-pareto-principle/
 [13]: http://stackoverflow.com/questions/8291366/how-to-benchmark-efficiency-of-php-script
 [14]: http://www.phpbench.com
 [15]: http://phpperf.com
 [16]: http://php.net/docs.php
