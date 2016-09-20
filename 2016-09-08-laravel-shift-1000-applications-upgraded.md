---
layout: post
title: "Laravel Shift - 1,000 applications upgraded"
date: 2016-09-08 11:37
comments: true
categories:
  - Main Thread
description: 'A retrospective on creating Laravel Shift as it reaches its milestone of 1,000 Laravel applications upgraded.'
subheading: 'A retrospective on creating Laravel Shift as it reaches its milestone of 1,000 Laravel applications upgraded.'
excerpt: 'A retrospective on creating Laravel Shift as it reaches its milestone of 1,000 Laravel applications upgraded.'
---
[Less than a year ago I created Laravel Shift](/2015/11/laravel-automated-upgrade-tool-shift/). While not my first product, it is my first software as a service (SaaS). If you're not familiar with [Laravel Shift](https://laravelshift.com) or interested in the backstory check out the [Q&A on Laravel News](https://laravel-news.com/2016/01/automatically-upgrade-your-laravel-app-with-shift/) or listen to the [interview on Full Stack Radio](http://www.fullstackradio.com/36).

In this post, I want to focus more on reaching the milestone of 1,000 Laravel applications upgraded. This may not sound like many, however for my first SaaS product it marks the achievement of my *stretch goal*. So allow me to share the most important decision, biggest challenge, and what the future holds for Laravel Shift.

## A product, not a project

Like many developers I have dozens of personal projects. Some I work on, some I don't, most I never complete. That's normally okay because if nothing else these are learning opportunity.

To that point, that's something my personal projects have taught me - you have to distinguish between a *project* and a *product*. This can be tough because we're passionate about our ideas. As such were willing to spend countless hours trying to bring them to life. It's easy to think, "Who cares? It's just my time." But when treated as a product, we start to value our time. Because as a creator *your* time is the *most* valuable.

I took this even further by treating Shift not only as a product, but a [minimum viable product](https://en.wikipedia.org/wiki/Minimum_viable_product) (MVP). Initially Shift only supported upgrading to the latest version of Laravel (upgrading from 5.0 to 5.1). I remember during this initial release, [Jeffrey Way](https://twitter.com/jeffrey_way) tried it out on [Laracasts](https://laracasts.com) and reported Shift as a "cool tool", but "somewhat *buggy*".

Such feedback coming from a big name in the Laravel community could be crushing. But not for Shift. Because I knew I built an MVP. Its features were deliberately limited until I proved the product. Proof came in preset milestones of 100, 250, 500, and 1,000 Shifts. Each time Shift reached a milestone, I would spend more time either fixing bugs, adding minor features, or releasing new Shifts.

I feel the decision to adopt an incremental, measured approach has been the most important, and provided the foundation to continue to grow Shift.

## A developer, not a marketer

As with any product, marketing can be a challenge. Being a programmer, I carry with me the stigma of social awkwardness. The last thing I'm comfortable doing is peddling my product to the public. Fortunately, Shift has provided personal recognition within the Laravel community. This has allowed me to appear on podcasts and speak at [Laracon](http://laracon.us). So I am thankful to be given these opportunities to mention Shift.

Shift also has a marketing sub-challenge. Many say Shift "should cost more". This has been my biggest challenge. On one hand, charging more would increase revenue. On the other hand, increasing price could slow growth. I'm not sure what's best. So, I do what I'm comfortable with.

I have risen prices slightly, particularly for shifting older versions of Laravel. Which really I consider an incentive to stay  up-to-date. I also [accept donations](https://laravelshift.com/donate). Although there have been a few, I realize getting more money after-the-fact is a low probability. Nonetheless, it's available for those wanting to show their appreciation or who use Shift commercially.

I expect pricing will always be a challenge. For now, I'd rather users say Shift "should cost more" than say Shift "costs too much".

## A beginning, not an end

So now that Shift has achieved its initial milestones, what's next?

Generally, the next milestones will be 2,500, 5,000, and 10,000 Laravel applications upgraded.

More specifically, a redesign for [laravelshift.com](https://laravelshift.com) is already underway. It will include a dashboard for managing your Shifts as well as streamlining the purchase process.

In addition, I'm formally announcing a set of [human services from Laravel Shift](https://laravelshift.com/human-services). While these have always been available, they were briefly mentioned on the FAQ page and only offered by request. Soon you will be able to purchase then just as you would a Shift.

Finally, I'll begin development on the most requested feature - support for upgrading Laravel Packages. Ideally, this will launch with the redesign or shortly after. Support for Lumen has been requested, but my MVP approach forces me to prioritize Laravel Packages first.

So, until the next milestone, keep shifting!
