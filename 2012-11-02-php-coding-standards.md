---
title: 'Be a Better PHP Developer: Coding Standards'
author: Jason McCreary
excerpt: >
  The next installment of a series on how to be a better PHP developer, and better
  developer in general. This post covers PHP coding standards.
layout: post
comments: true
permalink: /2012/11/php-coding-standards/
categories:
  - Main Thread
tags:
  - development
  - php
description: The next installment of a series on how to be a better PHP developer, and better developer in general. This post covers PHP coding standards.
---
In the last article of [this series][1] we cover the basics of how to [be a better PHP developer][2]. Now I want to cover coding standards.

Coding standards vary greatly among developers. Even when writing the same code. And therein lies the problem. This becomes most evident when reviewing code that is not your own.

My own coding standards have changed over the years. As team lead, I drafted coding standards documents. This time, I wanted something active and easily adopted.

## PHP coding standards

The [PEAR coding standard][3] is arguably the most prevalent among the PHP community. However, it is exhaustive and therefore not easily adopted. When reading about [PHP namespacing][4] last year, I came across the PHP Standard Requirements (PSR).

The latest version of this standard is [PSR-2][5], which expands [PSR-1][6], is a straightforward document outlining basic coding standards. It leaves some flexibility for [developer style][7]. And while I may not agree with every standard – mainly the curly brace conventions – it passes the governance.

## Stick to the coding standards

It is human nature to bend the rules. But there&rsquo;s no point in adopting a standard only to break it. I wanted to follow PSR-2 strictly. Not just the parts I liked. So I needed something to keep me honest.

A while back I came across PHP CodeSniffer. I intended to use it for automated code validation with an svn post-hook. PHP CodeSniffer features pluggable coding standards you can validate against. In addition, one for PSR-2 exist.

You can download PHP CodeSniffer. If you have [PEAR installed][8], it&rsquo;s easier:

    pear install PHP_CodeSniffer

Verify PHP CodeSniffer with:

    phpcs --version

**Note:** If you receive *warnings*, your PHP `include_path` likely does not include the PEAR directory. [Check your PEAR installation][9] to resolve this issue.

You can run PHP CodeSniffer against an entire directory.

    phpcs --standard=PSR2 api/

## Format legacy code

Since I just adopted the PSR-2 coding standard, I had several violations in my current projects. However, as traditional lazy developer, I didn&rsquo;t want to edit hundreds of lines of code.

I found [PHP CS Fixer][10]. It auto-formats code to meet PSR-2 (among others). While it doesn&rsquo;t correct everything, it fixes the tedious ones.

    php-cs-fixer fix api/ --level=psr2 --dry-run

**Note:** I added `--dry-run` for demonstration purposes. Remove it to update your files.

## Go forth and be a better PHP developer

A good developer should follow a coding standard. While I am advocating PSR-2 here, it is really up to you. What matters is that you follow it strictly. Hopefully the tools above can help you do so.

*This post is part of a series on [How to be a Better PHP Developer][1].*

 [1]: http://www.google.com/search?q=site%3Ajason.pureconcepts.net&q=%22Be+a+Better+PHP+Developer%22 "How to be a Better PHP Developer"
 [2]: http://jason.pureconcepts.net/2012/08/better-php-developer/
 [3]: http://pear.php.net/manual/en/standards.php "PEAR Coding Standard"
 [4]: https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-0.md
 [5]: https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-2-coding-style-guide.md
 [6]: https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-1-basic-coding-standard.md
 [7]: http://jason.pureconcepts.net/2012/09/code-style-fashion/
 [8]: http://jason.pureconcepts.net/2012/10/install-pear-pecl-mac-os-x/ "Install PEAR on Mac OS X"
 [9]: https://pear.php.net/manual/en/installation.checking.php
 [10]: https://github.com/fabpot/PHP-CS-Fixer
