---
layout: post
title: "Fake it by stubbing the mock, dummy"
date: 2017-11-14 1:03
comments: true
categories:
  - Main Thread
description: 'Why differences in test objects are often inconsequential and how abstraction can improve developer happiness.'
subheading: 'Why differences in test objects are often inconsequential and how abstraction can improve developer happiness.'
excerpt: 'Why differences in test objects are often inconsequential and how abstraction can improve developer happiness.'
---
I recently lead a workshop at ZendCon titled _Start testing your PHP Code_. I have given this workshop a few times. What I continually receive questions about are the different types of _mocks_ (testing objects).

There are indeed differences. I often borrow the definitions from Martin Fowlers [TestDouble](https://martinfowler.com/bliki/TestDouble.html) post (who borrows from [Gerard Meszaros](https://martinfowler.com/books/meszaros.html))

- **Dummy** objects are passed around but never actually used. Usually they are just used to fill parameter lists.
- **Fakes** objects actually have working implementations, but usually take some shortcut which makes them not suitable for production (an `InMemoryTestDatabase` is a good example).
- **Stubs** provide canned answers to calls made during the test, usually not responding at all to anything outside what's programmed in for the test.
- **Spies** are stubs that also record some information based on how they were called. One form of this might be an email service that records how many messages it was sent.
- **Mocks** are pre-programmed with expectations which form a specification of the calls they are expected to receive. They can throw an exception if they receive a call they don't expect and are checked during verification to ensure they got all the calls they were expecting.

When you're new to testing, all of this is clear as mud. Often compounded by the fact that these terms are used inconsistently across different testing frameworks.

The differences boil down to slight variations in implementation and usage. For example, the difference between a _fake_ and a _stub_ may simply be the _fake_ has an underlying class. Or the difference between a _spy_ and _mock_ is that a _spy_ also records invocations.

In my opinion, these differences are inconsequential. Often, they only matter when the testing framework makes this differentiation. Unfortunately, Mockery (the mock object framework I demo in my workshop) makes this differentiation.

In Mockery _stubs_, _mocks_, and _spies_ are all the same thing. Although it attempts to differentiate between spies and mocks, you still can make verifications on mocks. They only real difference is in their default behavior. A Mockery _mock_ requires you to stub every method used during the test. Whereas a spy does not. If you call a method on a spy in Mockery, it will simply return `null`.

In Mockery, there is also the ability to create a _partial mock_ which behaves more like a _fake_, as it will call through to the implemented (original) method if you do not stub it.

Again, clear as mud.

I like the generic term by Gerard Meszaros - test double. RSpec actually follows this by simply using `double()` to create _an object that stands in for another object in your system_.

That's simple enough. When testing, I don't _really_ care about the differences between test objects. I only care that I can easily create and use a test object.

In the end, although there are indeed differences between test objects, I don't want to be hindered by them while testing. It comes down to developer happiness. _Don't make me think_!

_**Using Mockery?** I created a simple helper function for Mockery called [`double()`](https://github.com/jasonmccreary/test-double) to abstract all these nuances back to the general use of test doubles and make testing easier._
