---
layout: post
title: "Are you a Boy Scout?"
heading: "Are you a Boy Scout?"
date: 2015-01-01 11:37
comments: true
categories:
  - Rants
description: 'A look at how the simple practice of "boyscouting" can improve your code.'
subheading: 'A look at how the simple practice of "boyscouting" can improve your code.'
excerpt: 'A look at how the simple practice of "boyscouting" can improve your code.'
subheading: 'A look at how the simple practice of "boyscouting" can improve your code.'
---
About a year ago [Samuel Goodwin](https://twitter.com/samuelgoodwin) left a commit message of *"Boyscouting"*. When I asked him about the commit he reminded me of the Boy Scouts <s>motto</s> <sup>[[1]](#footnote1)</sup>:

> Leave it better than you found it.

Applied to development, this meant [eliminating dead code](http://en.wikipedia.org/wiki/Dead_code_elimination), removing comments, and standardizing format. Samuel did this before he made changes.

Since then, I have tried to follow this practice <sup>[[2]](#footnote2)</sup>. It requires discipline. Not only in routine, but in restraint. It's tempting to add other changes to your *"Boyscouting"* commit.

It is important to understand boyscouting does not *change* code, only *cleans* it. Boyscouting is not refactoring. Boyscouting is not fixing bugs.

When in doubt, see if your boyscouting passes this test:

> Would reverting the commit result in code loss?

If so, then you've done more than boyscouting. Commits are cheap. As shown in the screenshot, separate changes into their own commit.

<img src="/images/boyscouting-code-commit.png" alt="Boyscouting Commit" title="Boyscouting Commit" style="width: 100%;" />

Practicing something as simple as boyscouting not only improves the codebase, it improves my development. I no longer waste time on dead code or fixing formatting. Instead I can focus on making changes and use any extra time to improve the code further.

Share the practice of boyscouting with others as Samuel did with me. Boy Scout your code!

*If you're wondering about the icons next to the commit hashes check out [Mergeatron](http://snapinteractive.github.io/mergeatron/).*

<a name="footnote1">[1]</a> As pointed out by [**SwabTheDeck** on reddit](http://www.reddit.com/r/programming/comments/2s6sxm/do_you_boy_scout_your_code/cnn7j5p) and [**Neal** in the comments](#comment-1790158085) this is not the Boy Scout *motto*, but [a passage left by Robert Stephenson Smyth Baden-Powell](http://www.scouting.org/Home/CubScouts/Parents/About/history.aspx#ad), founder of *Scouting*.

<a name="footnote2">[2]</a> Upon researching the Boy Scout motto, I also came across *The Boy Scout Rule* in [97 Things Every Programmer Should Know](http://programmer.97things.oreilly.com/wiki/index.php/The_Boy_Scout_Rule).
