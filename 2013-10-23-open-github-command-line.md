---
layout: post
title: "gh - Open GitHub from the Command Line"
heading: "gh - Open GitHub from the Command Line"
date: 2013-10-23 21:53
comments: true
categories: 
- Main Thread
excerpt: "A shell script to open GitHub from the command line."
subheading: "A shell script to open GitHub from the command line."
description: "A shell script to open GitHub from the command line."
subheading: "A shell script to open GitHub from the command line."
---
At work we used git backed by [GitHub](https://github.com). After every `git push` I open the browser, go to GitHub, and navigate to the branch.

While only a few clicks, I repeat this process many times a day. In lazy developer fashion I wanted to automate this process.

A Google search for *open GitHub from command line* yielded two promising results.

First, [hub](https://github.com/github/hub), which not only opened GitHub from the command line but wrapped `git` entirely to provide even more features. `hub` looked awesome, but went well-beyond my needs.

The second result was a [bash function](http://jasonneylon.wordpress.com/2011/04/22/opening-github-in-your-browser-from-the-terminal/) that generates your GitHub URL and uses the Mac OS `open` command to launch the browser.

Unfortunately it didn't work. Being just a few lines of code, I decided to fix the script. To honor the original author, I kept the same command name - `gh`.

By simply typing `gh` I can open the current branch in my repository on GitHub.

I also extended the original script with optional parameters for *remote* and *branch*. It follows a similar format as `git push`:

    gh [remote-name] [branch-name]

These default to *origin* and the current checked out branch, respectively.

Now I can open any repository and branch on GitHub from the command line with:

    gh origin dev

I shared [`gh` on GitHub](https://github.com/jasonmccreary/gh). Please send me a pull request with your additions.
