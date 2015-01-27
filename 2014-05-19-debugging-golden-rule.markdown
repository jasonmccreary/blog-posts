---
layout: post
title: The Debugging Golden Rule
date: 2014-05-19 21:23
comments: true
categories:
- Main Thread
excerpt: Developers need to remember the Debugging Golden Rule.
description: Developers need to remember the Debugging Golden Rule.
---

> Don't question others until you've questioned yourself.

Developers, especially new developers, often forget this when debugging. We jump right into the profiler. We add tracing statements. We review the commit log.

Such actions can be misguided. Before debugging the code we must follow a code. Debugging needs a [Golden Rule](http://en.wikipedia.org/wiki/Golden_Rule). A rule to remind developers of a few important points while debugging.

## It's you, 99% of the time
I had an excellent TA for my introductory Computer Science course. He gave great advice for improving your craft. Some of which became the foundations for [routines of a good developer](/2009/12/good_developer_routines/ "Routines of a Good Developer").

In one of our more difficult labs he expressed his frustration with the class:

> Listen up! Stop asking me, "What's wrong with `gcc`?" `gcc` has been widely used for over a decade. You have been programming for half a semester. It's not `gcc`!

I remember his words anytime I start questioning others. A far majority of the time, the bug is in my code.

## Avoiding tunnel vision
Debugging is the art of asking the right questions. If you start by questioning others, you will likely waste your time asking the wrong questions.

We've all done it. A recent upgrade revealed a bug in the code. Now you have a bug in your brain: *it's the upgrade*.

Of course you should question the upgrade. But question everything else too.

## You still have to fix it
Let’s say you prove the language has a bug, Internet Explorer behaves differently, or the developer before you broke the build. What then?

Well, *you* still have to fix it. Even when it is not you, you still must develop the fix. No point in focusing on blame.

Following the [Debugging Golden Rule](/2014/05/debugging-golden-rule/ "The Debugging Golden Rule")  keeps developers grounded, focused, and part of the solution. Remember it the next time you have a bug in your code.




