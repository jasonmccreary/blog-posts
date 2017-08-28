---
layout: post
title: 'A closer look at git rebase'
date: 2017-02-14 19:37
comments: true
categories:
  - Git
description: "For some, git rebase falls on the magic end of the spectrum for Git commands. In this post, we'll take a closer look at git rebase."
subheading: "For some, git rebase falls on the magic end of the spectrum for Git commands. In this post, we'll take a closer look at git rebase."
excerpt: "For some, git rebase falls on the magic end of the spectrum for Git commands. In this post, we'll take a closer look at git rebase."
---
Most of the comments to my recent articles on [3 Git commands I use every day](https://jason.pureconcepts.net/2016/11/3-everyday-git-commands/) and [When to make a Git commit](https://jason.pureconcepts.net/2017/01/when-to-make-git-commit/) have mentioned using `git rebase`.

So, let's talk about `git rebase`. Jumping right in, I use `git rebase` for two reasons:

1. To bring a *stale* branch up to date.
2. To change a set of *unmerged* commits.

Let's take a closer look at both of these.

## Bringing a branch up to date

For some, `git rebase` falls on the *magic* end of the spectrum for Git commands. Yet, if we break down the actions taken by `git rebase` we can understand the magic.

While a *tree* is the goto analogy when visualizing Git commands, I find *video editing* also helps describe `git rebase`.

In the case of bringing a *stale* branch up to date, let's consider the following tree progression.

![Visual of commit tree during git rebase](/images/tree-progressing-git-rebase.png "Visual of commit tree during git rebase")

Starting with the full tree, we have a *stale* branch (in red) off a *master* branch. If we zoom in, we see the branch is *stale* because it's missing the recent commits from *master* (in blue).

When we run `git rebase`, it first will *rewind* both branches back to the first point when their commit history matches (in gray). From this point, `git rebase` will *fast-forward* through the commits on the *master* branch and apply them to the *stale* branch. Finally, `git rebase` *replays* the commits from the *stale* branch.

The resulting tree is as if you just created a new branch off *master* and made your commits. In doing so, `git rebase` facilitates a clean merge.

## Changing a set of commits

I also like to use `git rebase` to change a set of commits. Often these are quick commits I made on a *feature* branch I want to *clean up* before merging. Either by condensing commits or improving their commit messages.

To do so, I'll run `git rebase -i`. The `-i` stands for *interactive*, because `git rebase` allows you to edit the *commit list*.

The output looks similar to the output from `git log --oneline`. However, each commit is prefixed with a *command*. The comments contain a legend for each of the commands.

![Commit message from git rebase -i](/images/git-rebase-interactive-message.png "Commit message from git rebase -i")

I'll commonly use `r` to *reword* a quick commit and `f` to *fixup* a commit into the previous commit without changing the message. Although many people talk about *squashing a commit*, I use *fixup* far more often than *squash* as the latter requires an extra step of editing the commit messages.

Upon saving, `git rebase -i` will *replay* these commits using the commands you specified.

## A few caveats

- There can be *conflicts*.
- The *replayed* commits get a new commit SHA.

If you're making small, cohesive commits (as outlined in [When to make a Git commit](https://jason.pureconcepts.net/2017/01/when-to-make-git-commit/)) any conflicts should be easy to resolve.

Finally, it's important to note that `git rebase` will change the SHA of any *replayed* commits. So if you *shared* your commits with others or merged them into another branch, Git will no longer see these commits as being the same.

***Want to Master more Git commands?** This post was adapted from my video series [Getting Git](https://gettinggit.com). It contains over 50 videos covering Git commands as well as scenarios you'll encounter using Git every day. The [Master: git rebase](https://vimeo.com/201459481) is available on Vimeo.*
