---
layout: post
title: "Fixing PHP Errors"
heading: "Fixing PHP Errors"
date: 2013-05-21 09:04
comments: true
categories:
- Main Thread
excerpt: "A guide to interpreting and fixing PHP errors."
subheading: "A guide to interpreting and fixing PHP errors."
description: "A guide to interpreting and fixing PHP errors."
subheading: "A guide to interpreting and fixing PHP errors."
---
I contribute on [Stack Overflow](http://stackoverflow.com/users/164998/jason-mccreary "Jason McCreary on Stack Overflow") regularly. I see an alarming [number of questions](http://stackoverflow.com/search?q=%22php+error%22) about fixing PHP errors. These questions often pertain to very basic errors. Even with the error message, these users still need help.

I have developed with PHP for over a decade. During that time I've encountered nearly every error. This post covers how to interpret a PHP error as well as fixing common PHP errors.

We will parse the following PHP code and resolve the errors. There are three. Four depending on how you define *errors* (more on that later). For now, bonus points if you can find the other *error*.

    <?php
    echo 'Hello Errors!'
    if ($user->name) {
        echo 'It's time to stop writting errors ";
        echo $user->name, '!';

## Interpreting PHP errors
When we run this code, we receive the first error:

> PHP Parse error: parse error, expecting ',' or ';' in errors.php on line 3

Before we fix this error, let's interpret the error. PHP errors have a three important parts:

- **Error type** found at the beginning tells us the error type. PHP has many [error types](http://www.php.net/manual/en/errorfunc.constants.php "PHP error contants"). `Parse` or `Fatal` errors being more common. Example: *PHP Parse error*
- **Error message** provides us a hint about the error. Example: *expecting ',' or ';'*
- **Error context** tells us where the error occured. Example: *errors.php on line 3*

Together these parts provide all the information we need to fix our code.

## PHP Error #1: Expecting ',' or ';'

> PHP Parse error: parse error, expecting ',' or ';' in errors.php on line 3

The error tells us we have a parse error on line 3. Looking at line 3 again:

    if ($user->name) {
    
Seems correct. What's wrong?

This is where *error type* can help solve the mystery. For parse errors, the error typically occurs on the preceeding line since the parser continues until it reads invalid syntax. Let's look at line 2:

    echo 'Hello Errors!'

Now if you wrote this code, you may not see the error. In which case the error message provides a hint: *expecting ',' or ';'*.

Expecting a comma&hellip; What? `echo` allows you to output multiple *strings* separated by commas. However, this was not our intention.

Expecting a semi-colon&hellip; Ahh. The line is missing its required semi-colon line ending. Let's fix the error by adding a semi-colon to the end of line 2.

## PHP Error #2: Unexpected T_STRING
> PHP Parse error: unexpected T_STRING in errors.php on line 4

Another parse error. Applying what we've learned, we look at line 4.

    echo 'It's time to stop writting errors ";

Let's examine the *strings* on this line. The intended string was: *It's time to stop writting errors*. In PHP strings are quoted. Using the quotes, we see that our string is really *It* then *s time to stop writting errors*.

We confused ourselves, and PHP, by starting with a single quote and closing with a double quote, while the string contains an apostrophe (single quote). 

To fix this, we can either start the string with a double quote or use single quotes throughout and escape the apostrophe:

    echo 'It\'s time to stop writting errors ';

These errors can be difficult to spot. Often syntax highlighting helps. If your IDE doesn't have syntax highlighting, please swtich IDEs. Even this blog post has syntax highlighting!

On a side note, there are many arguments between using [single-quotes versus double-quotes](/2012/12/php-micro-optimizations/ "PHP Micro Optimizations") in PHP. Allow me to end this - it doesn't matter. What does matter is consistency. Personally, I use single-quotes everywhere.

## PHP Error #3: Unexpected end of file
> PHP Parse error: unexpected end of file in errors.php on line 7

Another parse error. The fact that line 7 does not exist reminds us to look at the preceeding line for parse errors.

    echo $user->name, '!';
    
This line seems fine. Let's keep going up a line until something looks wrong. Now I've written enough PHP to know that this particular error message deals with unterminated syntax. Meaning that the perser was expecting more syntax, but instead reached the end of the file.

Knowing this, I know the error relates to following line:

    if ($user->name) {
    
We never closed the `if` block. Adding the closing brace, `}`, on line 7 fixes the error.

    if ($user->name) {
        echo 'It's time to stop writting errors ";
        echo $user->name, '!';
    }

[Formatting your code](/2012/11/php-coding-standards/ "PHP Coding Standards") goes a long way to prevent these errors. Generally speaking, if you reach the end of a code block at an indentation level you forgot to terminate something. Most IDEs have auto-indentation features. Configure indendation and choose your side in the battle between [tabs and spaces](http://nithinbekal.com/2011/tabs-vs-spaces-for-indentation/).

## Treating warnings like errors
Our code now runs without errors. But there are a few warnings. Often warnings are errors that haven't happened yet. But under the right edge case they will, and when they do, your code will fail. As such, many developers treat warnings like errors.

There are also notices. Since PHP is a dynamic language, I often don't treat notices as errors. But notices can indicate just as much danger as a warning. Over the years, I have slowly treated notices as errors. There is something to be said for PHP code that contains no errors, warnings, or notices.

Now fix your errors and write pure code.



