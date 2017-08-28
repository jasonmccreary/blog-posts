---
layout: post
title: 'Using git bisect'
date: 2017-04-12 11:37
comments: true
categories:
  - Git
description: 'A real world example of using git bisect to find a bug deep in my application code.'
subheading: 'A real world example of using git bisect to find a bug deep in my application code.'
excerpt: 'A real world example of using git bisect to find a bug deep in my application code.'
---
A few weeks back I gave a talk at [Laracon Online](https://laracon.net) entitled *You don't know Git*. During the talk I demoed several unfamiliar Git commands. While I did cover `git bisect`, I didn't provide a demo.

Demoing `git bisect` is challenging. The command has several subcommands and requires context about the code. As such, a contrived example doesn't do `git bisect` justice.

Now, I've admittedly only used `git bisect` five or six times in my Git career. But just the other day I used it to find a bug deep in my application. I want to share this scenario as a real world example of using `git bisect`.

## The context
First, a little context. As I'm sure you have noticed, I recently released a video series on Git called [Getting Git](https://gettinggit.com). I use [Stripe Checkout](https://stripe.com/docs/checkout/tutorial) to handle payments. This keeps things pretty simple, just requiring me to drop a piece of JavaScript on the page. Stripe injects the necessary front-end code to create a pay button that pops up a checkout form.

The other day a someone went to checkout and when they clicked the pay button *nothing happened*. Fortunately they reported this, so I started investigating. I confirmed the bug in a few different browsers. I reached out to Stripe support. There were no known issues on their side. So I dropped their checkout code onto a fresh page and it worked fine.

The bug was clearly in my code. But *where*? Looking at the commit log, there wasn't anything related to the checkout code in a while. In fact, using `git blame` showed that the checkout code hadn't changed in over a month. I knew this bug hadn't existed for that long.

While I could've continued this detective work myself and eventually found the bug, I could instead use `git bisect`.

## The command
Basically, you provide `git bisect` a commit where the bug exists (the *bad* commit) and a commit where it does not (the *good* commit). `git bisect` then systematically moves through this commit range to find the commit where the bug was introduced. Along the way it pauses to allow you to test for the bug. If the bug exists, you enter report the commit as *bad*. If it does not, you report it as *good*.

I took a few screenshots when I used `git bisect` to find my checkout bug. I'll review these to work through the process of using `git bisect`.

First I run `git bisect start` to start the bisect process.

I then provide the *bad* commit by running `git bisect bad`. In my case, this was the top commit as the bug currently appeared in my code.

I then provide the *good* commit by running `git bisect good`. In my case, I got this commit from `git blame` as I knew the checkout originally worked. Finding the *good* commit may take a little detective work. However, this doesn't have to be precise. That's what `git bisect` is for. Often, I'll jump back several commits at a time until I find a *good* commit.

![Starting git bisect](/images/starting-git-bisect.png "Starting git bisect")

Once I provide both commits, `git bisect` starts moving through this commit range. It pauses to allow me to test if the bug exists in the current commit. If so, I type `git bisect bad`. If not, I type `git bisect good`.

![Testing git bisect](/images/testing-git-bisect.png "Testing git bisect")

After a few iterations, `git bisect` outputs the first *bad* commit. Or the commit where the bug was introduced.

![Output of git bisect](/images/result-git-bisect.png "Output of git bisect")

Honestly, the first few times I used `git bisect` I didn't believe this was the right commit. But I assure you there's some change in this commit that introduced the bug.

In my case, I added some JavaScript that listened for *click* events. It was preventing the default behavior and, as such, preventing the checkout form from launching.

`git bisect` is not a command you'll use very often. Nonetheless, it's one of the most helpful and impressive Git commands. Next time you have a bug in your code, don't play try to find the need in the haystack of your commit history - use `git bisect`.

_**Want to see `git bisect` in action?** I demo `git bisect` and other Git commands you'll use every day in my video series [Getting Git](https://gettinggit.com)_
