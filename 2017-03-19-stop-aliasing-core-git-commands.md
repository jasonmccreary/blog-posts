---
layout: post
title: 'Stop aliasing core Git commands'
date: 2017-03-19 11:37
comments: true
categories:
  - Git
description: 'Some claim aliasing core Git commands is the "Right Way". It''s not.'
subheading: 'Some claim aliasing core Git commands is the "Right Way". It''s not.'
excerpt: 'Some claim aliasing core Git commands is the "Right Way". It''s not.'
---
A core feature of Git is the ability to create *aliases*. This effectively allows you to customize Git's command set. As a developer, of course, you're going to want to do this.

However, lately I've come across numerous claims stating aliasing core commands is the *Right Way* to use Git. Unfortunately, even [Pro Git](https://git-scm.com/book/en/v2/Git-Basics-Git-Aliases) aliases core Git commands in their examples. Regardless, this is **not** the *Right Way*.

Why?

Two reasons: obfuscation and speed.

## Obfuscation
While aliases give us freedom, there's no convention for aliasing core commands. So they're all subjective.

![Sample aliases of core Git commands](/images/aliases-core-git-commands.png "Sample aliases of core Git commands")

While these commands exhibit our personal flare, they've lost their meaning. Sure `git up` sounds cool and might impress your coworkers. But they have no idea what it does and it isn't available on their setup.

## Speed
The primary motivation for aliasing core commands is speed. Oh, the need for speed. Anything to save a few keystrokes. But how many keystrokes are you *really* saving by aliasing core Git comamnds?

Let's compare some common aliases against command completion.

![Keystroke comparison between aliases and command completion](/images/keystrokes-git-aliases-vs-command-completion.png "Keystroke comparison between aliases and command completion")

With the exception of `git status`, command completion tied or beat aliases. In addition, command completion also completes references and options. So command completion saves keystrokes across all commands, not just aliases.

In the end, aliases are a useful feature. But stop aliases core Git commands. Instead, use command completion as a clearer and often faster alternative.

Reserve aliases for Git commands you run frequently and require options. For example, here are my current aliases. Two alias long `git log` commands and the others compliment Git's command set with additional custom commands.

![My current Git aliases](/images/jmac-git-aliases.png "My current Git aliases")

_**Want to use Git the "Right Way"?** [Getting Git](https://gettinggit.com) contains over 60 videos covering Git commands as well as scenarios you'll encounter using Git every day._
