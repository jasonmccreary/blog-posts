---
layout: post
title: Committing to the wrong branch
date: 2017-09-05 1:03
comments: true
categories:
  - Git
description: 'Exploring the various Git commands you can run when you make a commit on the wrong branch.'
subheading: 'Exploring the various Git commands you can run when you make a commit on the wrong branch.'
excerpt: 'Exploring the various Git commands you can run when you make a commit on the wrong branch.'
---
Lately [CodeRabbi](https://twitter.com/coderabbi) has been tweeting some Git aliases. We all know where [I stand on aliases](https://jason.pureconcepts.net/2017/03/stop-aliasing-core-git-commands/). Be that as it may, [his recent tweet](https://twitter.com/coderabbi/status/904708039533584387) received a lot of replies proposing different solutions.

This contributes to the steep learning curve with Git - what's the _proper_ way to do something? I try to address in [Getting Git](https://gettinggit.com) by showing different ways Git commands may be used.

In this case, an alias of `move` was created to solve the problem of committing work to an incorrect branch. It did so by running the following commands.

```git
MESSAGE=$(git log -1 HEAD --pretty=format:%s)
git reset HEAD~ --soft
git stash
git checkout destination-branch
git stash pop
git add .
git commit -m $MESSAGE
```

As mentioned in the replies, there are other ways to solve this problem.

Before exploring alternative solutions, I want to address the ambiguity of the problem. In order to determine the _proper_ solution, we need to answer a few questions:

1. Does the _destination_ branch exist or does it need to be created?
2. Did I commit on top of work that *does not* belong on the _destination_ branch?
3. How many commits did I make _incorrectly_?

For me, _proper_ means using commands with side-effects so I expend the minimal effort.

By that definition, I immediately rule out the use of `stash`. While `stash` is a helpful Git command, it is very nuanced. For example, it _stashes_ everything in the index, but not untracked files. This may not be what you want. Maybe you just want staged changes. Maybe you do want untracked files.

There's also no need to reset the commit just to recommit it on another branch. The assumption being you want the commit _as is_. You just made it on the wrong branch.

Let's look at some alternative solutions.

## checkout + reset
A straightforward solution is to simply create another branch from your current branch. It will have the same commit history and therefore contain the incorrect commit. This allows you to remove the commit from the current branch. Then you can checkout the new branch and complete your work.

```git
git branch destination-branch
git reset --hard HEAD~1
git checkout destination-branch
```

### Assumptions
This makes the assumption that the destination branch *does not* exist. It also assumes all previous commits belong on the destination branch. If either of these assumptions are not true, use the `cherry-pick` solution.

This also assumes you only made one commit _incorrectly_. If you made more, increase the relative reference accordingly (e.g. `HEAD~2`, `HEAD~3`, etc)

## cherry-pick
Another solution is to use `cherry-pick`. As noted above, _cherry picking_ may be used when the commit histories differ or the destination branch already exists.

First, you create and checkout the destination branch. You pass the second argument to `checkout` to reference a branch point since the branches don't have the same commit history. Then you can `cherry-pick` the incorrect commits. Once done, you can switch back to the previous branch and reset the incorrect commits.

```git
git checkout -b destination-branch good-reference
git cherry-pick 12345
git checkout -
git reset --hard HEAD~1
```

### Assumptions
This assumes the destination branch *does not* exist. If it does, change the first command to: `git checkout destination-branch`.

As with other solutions, if you made more than one commit incorrectly, you will need to run `cherry-pick` for each of the incorrect commits. You may also pass `cherry-pick` a range as of Git 1.7.2. For example, if `eddd21` referenced your first incorrect commit and `7e6802` referenced the last, you could run: `git cherry-pick eddd21^..7e6802`. Cherry picking a range has its own nuances. I often find I just run the individual `cherry-pick` commands.

## push + reset
In some scenarios, you may be able to simply push your current branch to a remote branch of a different name. Then reset your local branch to remove the incorrect commit.

```git
git push origin HEAD:destination-branch
git reset --hard HEAD~1
```

### Assumptions
This assumes you are working with remote branches. It also assumes the commit histories are the same, but it *does not* matter if the destination branch exists or not.

As with other solutions, adjust the relative reference used in the `reset` command to remove the appropriate number of commits.

## Conclusion
You can see why Git can be challenging to learn and use. There are at least four different solutions for this one problem. While I advocate for a solution that keeps the commands simple, I hope by demonstrating all solutions you learned when to apply each.

_**Want to see more every day Git scenarios?** In addition to learning about core commands, the [Getting Git](https://gettinggit.com) video series also demonstrates the commands you'll use to solve every day problems with Git._
