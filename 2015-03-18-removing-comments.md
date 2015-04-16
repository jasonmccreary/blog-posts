---
layout: post
title: "Removing Comments"
date: 2015-03-18 11:37
comments: true
categories:
  - Main Thread
description: "A closer look at the practice of removing comments from code."
subheading: "A closer look at the practice of removing comments from code."
excerpt: "A closer look at the practice of removing comments from code."
---
My recent post on [Boyscouting](http://jason.pureconcepts.net/2015/01/are-you-a-boy-scout/) received 15 minutes of fame on the [programming subreddit](http://www.reddit.com/r/programming/). I found most of the discussion focused on code comments. Specifically, that part of boyscouting was *removing* comments.

This came up again in a review of the code:

    // filter out contacts that have unsubscribed
    $contacts = $unsubscribedFilter->filter($contacts);

    // filter out duplicate contacts
    $contacts = $duplicateFilter->filter($contacts);

The reviewer asked why I "*removed the documentation*" when I condensed the code to:

    $contacts = $unsubscribedFilter->filter($contacts);
    $contacts = $duplicateFilter->filter($contacts);

I'll come back to the difference between *comments* and *documentation*. For now, realize there is a difference and what I removed were indeed comments.

First, let me acknowledge all the team leads and enginering managers crying out:

> Remove Comments?!!

Yes, I am suggesting removing comments from your code.

> Surely you don't mean removing useful comments?

Well, what's a *useful* comment?

Let's back up and address the distiction between *comments* and *documentation*. Documentation is formatted comment blocks (e.g. DocBlock, JavaDoc, etc). Comments are everything else.

A comment should relay **why**, not what or how. Said another way, if there is something that can't be relayed by reading the code, then a comment may be needed.

Going back to the code review, there was nothing the comments relayed that the code did not. I can infer from the assignment and method names we are "*filtering duplicate contacts*". So the code comment above is **not** useful. In fact, I wasted time reading it.

For me, removing comments is about acheiving code that clearly communicates. One could even refactor the code to improve readability. Consider:

    $contacts->filterUnsubscribed();

Comments can not only be useful, they can also be misleading. I continually come across outdated comments that have not evolved as the code has changed. I recently needed to fix the following legacy code which was prematurely ending.

	foreach ($items as $item) {
		if ($item->published) {
			// we've hit the most recent item before this push, so stop looping
			exit;
		}
	}

The comment says to stop looping, but the code exits. I wasted several minutes debating which to trust. Given the bug, I updated the code to follow the comment and *stop looping*. Regardless, this bug would have been solved without the comment. Combined with the buggy code, it did more harm than good.

That's really what it's about – doing good. Leaving something better than you found it. That's why it's called [Boyscouting](http://jason.pureconcepts.net/2015/01/are-you-a-boy-scout/). If you come across a comment that you can remove, remove it. If you can't remove it, see if you can refactor the code so you can remove the comment. Future developers will thank you. Even if that future developer is you.

***Update:** I recently came across the following passage from [Rob Pike](https://twitter.com/rob_pike) regarding comments which, quite effectively, summarizes this entire post.*

> [comments are] a delicate matter, requiring taste and judgment. I tend to err on the side of eliminating comments, for several reasons. First, if the code is clear, and uses good type names and variable names, it should explain itself. Second, comments aren’t checked by the compiler, so there is no guarantee they’re right, especially after the code is modified. A misleading comment can be very confusion. Third, the issue of typography: comments clutter code.
