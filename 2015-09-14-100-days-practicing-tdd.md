---
layout: post
title: "100 days practicing TDD"
date: 2015-08-21 19:37
comments: true
categories:
  - Main Thread
description: "A retro after practicing test driven development for 100 days."
subheading: "A retro after practicing test driven development for 100 days."
excerpt: "A retro after practicing test driven development for 100 days."
---
I recently joined an [extreme programming team](/2015/05/two-weeks-extreme-programming/) at the [Humana DEC](http://www.humana.io). Every workday since I have practiced test driven development (TDD). After 100 days, I want to point out some differences between TDD *in theory* and TDD *in practice*.

So with respect to the [Three Laws of TDD](http://programmer.97things.oreilly.com/wiki/index.php/The_Three_Laws_of_Test-Driven_Development) here are my caveats:

1. You don't need to test *everything*
2. You can write more than one failure at a time
3. You don't need to practice TDD at the *nano cycle*
4. You should delay design decisions until the blue phase
5. You should refactor tests too

Before you punch your screen allow me to elaborate.

## You don't need to test *everything*
In theory, and according to the first law of TDD:

> You can't write any code until you have first written a failing test.

In practice, I rarely write tests for content, design, configuration, etc. I write tests for any code that contains logic.

## You can write more than one failure at a time
In theory, and according to the second law of TDD:

> You can't write more of a test than is sufficient to fail.

In practice, I often write a few failures at a time. However, these are typically within the same test and always at the same level. That is a few unit tests failures or a few integration tests failures. Then I make them pass one by one.

## You don't need to practice TDD at the *nano cycle*
In theory, and according to the third law of TDD:

> You can't write more code than is sufficient to pass the currently failing test.

In practice, I follow TDD Law #2 and #3 when working with a new codebase or new technology. Once I am familiar, I write the failing test and code to pass in one cycle. I see no need to repeat the red-green cycle at the *minimal* pace <sup>[[1]](#footnote1)</sup>.

## You should delay design decisions until the blue phase
In theory, as noted in the third law of TDD, the green phase is about writing minimal code to make the test pass.

In practice, many people refactor during the green phase (or earlier). This is too early. To avoid refactoring during the green phase I call [YAGNI](http://martinfowler.com/bliki/Yagni.html) on nearly everything. Delay design decisions until the blue phase. By then you'll have a better understanding of the code and tests to guide your refactor.

## You should refactor tests too
In theory, *all* code should be refactored.

In practice, tests are rarely refactored. Tests are code too and should be refactored during the blue phase. Futhermore, when practicing TDD, tests serve as documentation. It is therefore equally, if not more important that you ensure the test code communicates clearly.

---

<a name="footnote1">[1]</a> While writing this post, I found a post by Uncle Bob in which he discusses the [different TDD cycles](http://blog.cleancoder.com/uncle-bob/2014/12/17/TheCyclesOfTDD.html). Much of the *theory* above operates on the *nano cycle*. What I have described *in practice* combines mostly the *minute* and later cycles.