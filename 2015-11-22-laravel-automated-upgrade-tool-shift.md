---
layout: post
title: "Shift: Laravel Automated Upgrade Tool"
date: 2015-11-22 19:37
comments: true
categories:
  - Main Thread
description: "Introducing Shift - a Laravel framework automated upgrade tool."
subheading: "Introducing Shift - a Laravel framework automated upgrade tool."
excerpt: "Introducing Shift - a Laravel framework automated upgrade tool."
---
***Update:** An alpha release of [Laravel Shift](https://laravelshift.com) is now available.*

This past week I presented [*All Aboard for Laravel 5.1*](http://www.slideshare.net/mccreaja/all-aboard-for-laravel-51) at the [2015 PHP[world] conference](https://world.phparch.com/schedule/). This talk focused on the new features in Laravel 5.0 and steps to upgrade from Laravel 4.2.

In researching this talk, I only found one resource, aside from the official [Upgrade Guide](http://laravel.com/docs/5.1/upgrade), detailing the upgrade process. I was surprised to not find a Laravel upgrade tool. The changes from Laravel 4.2 to Laravel 5.0 were significant, yet straightforward and easily automated.

Fortunately [Taylor Otwell](https://twitter.com/taylorotwell), creator of the Laravel framework, was also at PHP[world]. He was not aware of any automated upgrade tool and expressed interest in such a tool.

So during the conference hackathon *Shift* was born. I wrote an initial *Shift* to automatically upgrade a Laravel  5.0 application to Laravel 5.1.

I was able to get a few alpha testers from [Taylor's initial tweet](https://twitter.com/taylorotwell/status/667520395952709632). However, I am still looking for additional alpha testers. If you have a Laravel 5.0 application and it's available through Git please [contact me](#about).

In the meantime, I am going to use [Laravel Spark](https://github.com/laravel/spark) to create a site where developers can log in with their GitHub account and easily submit their Laravel projects for automated upgrade. *Shift* will automatically upgrade the project and submit a pull request for review.

I hope to have the site live as well as a *Shift* ready for Laravel 5.2 release in December.

I welcome your feedback to gauge interest and request features. And please, [contact me](#about) to alpha test the Laravel 5.0 *Shift*. It's a free upgrade!