---
layout: post
title: "Git - Command line vs GUIs"
date: 2017-01-10 19:37
comments: true
categories:
  - Main Thread
description: 'A case for using Git from the command line instead of GUIs.'
subheading: 'A case for using Git from the command line instead of GUIs.'
excerpt: 'A case for using Git from the command line instead of GUIs.'
---

In the [introductory video for *Getting Git*](https://vimeo.com/album/4319966/video/196220914) I talk about using Git from the command line instead of through a GUI. I label GUIs as *evil wizards* and slay them with a custom `die` command.

But *why*? What's *wrong* with GUIs?

I didn't randomly label GUIs as *evil wizards*. This term comes from one of the first and still top programming books I've ever read – [The Pragmatic Programmer](https://pragprog.com/book/tpp/the-pragmatic-programmer).

*The Pragmatic Programmer* is filled with stories to highlight everyday scenarios you'll encounter as a programmer. Most of the stories end with a *Tip*. The book contains over 100 tips.

In this case, I'll reference *Tip 50*:

> Don't use wizard code you don't understand

This tip results from [*Section 35: Evil Wizards*](https://pragprog.com/the-pragmatic-programmer/extracts/wizards) and can be applied to the command line versus GUI debate. Since a GUI is generating Git commands on our behalf it classifies as a *wizard*.

So the question becomes, what's wrong with using a *wizard*?

I've adapted the following passage from the section:

> unless you actually understand the commands that have been produced on your behalf, you're fooling yourself. You're programming by coincidence... If the commands they produce aren’t quite right, or if circumstances change and you need to adapt the commands, you’re on your own.

I think this hits the main point. **You have to actually understand Git**. As programmers, it's a tool we use every day. One that has a very small set of commands - around 15, less than half of which you'll use frequently.

In the end, the problem isn't with the GUI, it's with a lack of understanding. I'll use a GUI for visual diffs or as a merge tool. But, I'm also comfortable viewing diffs and resolving merge conflicts from the command line.

If you're using a GUI for Git, I encourage you to challenge yourself. As you're using it, try to identify the underlying commands used to generate the current screen. If you can't, you're using an *evil wizard*.

***Want to be more comfortable using Git from the command line?** [Getting Git](https://gettinggit.com) contains over 40 videos covering Git commands from the command line as well as scenarios you'll encounter using Git every day.*
