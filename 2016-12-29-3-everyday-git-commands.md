---
layout: post
title: "3 Git Commands I use every day"
date: 2016-11-30 12:37
comments: true
categories:
  - Git
description: 'Some insight into three of the Git commands I use every day.'
subheading: 'Some insight into three of the Git commands I use every day.'
excerpt: 'Some insight into three of the Git commands I use every day.'
---
Lately I'm paying more attention to the Git commands I run to help script [Getting Git](https://gettinggit.com). I have so much material at this point, I was told to turn some of it into a blog post.

What I've found from speaking and [pairing](https://jason.pureconcepts.net/2016/04/mentoring-pair-programming-development-coaching/) is that most of us aren't as comfortable with Git as we might like to be. It's sharing insight into the Git command options and workflows that developers really seem to enjoy.

So here are three Git commands I use every day. Yes, I'm going to skip `git status`. No, they aren't Git commands you've never heard.

## 1: git add -p
So you've been hacking away all day on some changes, then you add all of them with `git add .` or `git add -A`.

Oh, the irony.

Leaving aside the heavy handed nature of these commands, you spent all that time crafting changes just to throw them in the Git repository. It's like preparing a nice meal, then shoving it all in your mouth at once.

When I complete my changes, I prefer running `git add -p`. The `-p` stands for *patch*. Running this command allows you to interactively review each of your changes. In doing so you can decide if you want to add the change or not.

While more time consuming than `git add .`, it's well worth it. More often than not I find some changes I probably don't want to commit -- a comment or debug statement. `git add -p` provides a final review to ensure my changes are ready to be committed.

## 2: git commit \-\-amend \-\-no-edit

When working on something, I usually only make one or two commits total. However, given the 10x ratio of reading code versus writing code these commits might happen over a long period of time. In between that time I like to make small, incremental commits -- even if it's just a *WIP* commit.

For these commits, I like to use `git commit --amend --no-edit`. Yes, I know I can use `git rebase` instead. But most of the time work is done sequential. So I don't need to need all that, I can simply append my changes to the previous comment.

This also allows me to keep a *clean state*, which makes it easier to run other Git commands as well as keep track of my changes over time. It also provides the freedom to *spike* on some changes that I may not end up using without relying on <kbd>⌘</kbd>-<kbd>z</kbd>.

I'll admit, this starts to get into what I consider a commit (which I'll wrote about in my [next post](https://jason.pureconcepts.net/2017/01/when-to-make-git-commit/)).

## 3: git reset \-\-hard

I know a few of you just freaked out. Some people find `git reset` scary, especially when run with the `--hard` option. However I find it complements the other two commands. For example, if there were changes I didn't add with `git add -p` or changes from a *spike* I didn't want to commit I can easily discard them with `git reset --hard`.

Now, that's not to say you should be cavalier with `git reset --hard`. There are still some [scary stories](https://www.reddit.com/r/git/comments/5hhmoi/undo_git_reset_hard_on_a_repo_with_no_commits/?). Simply being mindful will mitigate this fear. In reality, you should never be afraid to run a Git command when armed with `git reflog` and `git fsck`.

In the end, I think these 3 Git commands say more to my everyday workflow. Maybe you see how they fit into your own. I explain these and over 20 other Git commands in my upcoming video series *Getting Git*. Which is now [available in early access](https://gettinggit.com/). And check out my next post on [when I make commits](https://jason.pureconcepts.net/2017/01/when-to-make-git-commit/).
