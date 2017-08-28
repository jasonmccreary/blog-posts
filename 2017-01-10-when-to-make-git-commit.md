---
layout: post
title: "When to make a Git commit"
date: 2017-01-10 19:37
comments: true
categories:
  - Git
description: '2 rules I follow for when to make commits.'
subheading: '2 rules I follow for when to make commits.'
excerpt: '2 rules I follow for when to make commits.'
---

You don't have to look through too many commit histories on GitHub to see people are pretty terrible about making commits.

![A sadly common commit history](https://jason.pureconcepts.net/images/git-commit-history.png)

Now I'm not going to talk about writing commit messages. Here's *the* [post](http://chris.beams.io/posts/git-commit/) on that. I want to talk about the equally important topic of *when* to make commits.

I get asked this a lot at conferences. Enough to where I made two *rules* I've continually put to the test.

I make a commit when:

1. I complete a *unit of work*.
2. I have changes I may want to *undo*.

Anytime I satisfy one of these rules, I commit that set of changes. To be clear, I commit **only** that set of changes. For example, if I had changes I may want to undo **and** changes that completed a unit of work, I'd make two commits - one containing the changes I may want to undo and one containing the changes that complete the work.

I think the second rule is pretty straightforward. So let's tackle it first. Over the course of time, you'll make some changes you *know* will be undone. Be it a promotional feature, patch, or other temporary change someday soon you'll want to undo that work.

If that's the case, I'll make these changes in their own commit. This way it's easy to find the changes and use `git revert`. This practice has proven itself time and again, I'll even commit changes I'm simply uncertain about in their own commit.

So back to the first rule.

I think generally most of us follow this rule. However, you don't have to scroll through too many repositories on GitHub to see that we're pretty bad with commits.

The discrepancies come from how we define a *unit of work*.

Let's start with how **not** to define a *unit of work*.

A unit of work is *absolutely not* based on time. Making commits every X number of minutes, hours, or days is ridiculous and would never result in a *version history* that provides any value outside of a chronicling system.

Yes, *WIP* commits are fine. But if they appear in the history of your *master* branch I'm coming for you!

A unit of work is *not* based on the type of change. Making commits for *new files* separate from *modified files* rarely makes sense. Neither does any other *type* abstraction: code (e.g. JavaScript vs HTML), layer (e.g. Client vs API), or location (e.g. file system).

So if a *unit of work* is **not** based on *time* or *type*, then what?

I think it's based by *feature*. A feature provides more context. Therein making it a far better measurement for a *unit of work*. Often, implicit in this context are things like *time* and *type*, as well as the *nature* of the change. Said another way, by basing a *unit of work* by feature will guide you to make commits that tell a story.

So, why not just make the first rule: *I make a commit when I complete a feature*?

Well, I think this a case where the *journey* matters. A *feature* can mean different things, even within the context of the same repository. A feature can also vary in size. With *unit of work*, you keep the flexibility to control the *size* of the *unit*. You just need to know how to *measure*. I've found by *feature* gives you the best commit.
