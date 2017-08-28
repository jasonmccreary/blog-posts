---
layout: post
title: 'Lumen is dead. Long live Lumen.'
date: 2017-02-27 11:37
comments: true
categories:
  - Main Thread
description: "Why I believe Lumen is dying and what the future holds for Lumen."
subheading: "Why I believe Lumen is dying and what the future holds for Lumen."
excerpt: "Why I believe Lumen is dying and what the future holds for Lumen."
---
You've already read the title, so I'll just say it, *I think Lumen is dying, if not already dead.*

Now let me tell you why…

## Limited adoption
As the creator of [Laravel Shift](https://laravelshift.com) I get a unique pulse on the Laravel community. Something I noticed is the ratio of Laravel to Lumen Shifts is pretty staggering - about 500 to 1. At first, you may attribute this to selection bias. However, similar ratios can be found on [Packagist](https://packagist.org/).

The Laravel Framework currently has around 20M downloads, while the Lumen Framework has around 125k. That's a ratio of 160 to 1. Now these are overall numbers and Laravel has been out longer than Lumen.

Despite all these disclaimers, the download charts draw the same conclusion. The ratio between their *scale* is still 100 to 1.

![Download chart for Laravel Framework](/images/download-chart-laravel-framework.png "Download Chart for Laravel Framework")

![Download chart for Lumen Framework](/images/download-chart-lumen-framework.png "Download Chart for Lumen Framework")

## Less focus
Lumen also has a smaller feature set. To be fair, this is by design, as Lumen caters strictly to API development. However, while I personally agree with this direction, I think developers eventually find this restrictive. Once they hit this barrier, they are forced to switch to Laravel.

The shift to [stateless APIs in Lumen 5.2](https://lumen.laravel.com/docs/5.4/upgrade#upgrade-5.2.0) is a perfect example of this. Although this decision was clearly inline with the direction of the Lumen Framework, as a developer you were forced to limit your Lumen application or convert to a Laravel application.

I expect Lumen's feature set will continue to lessen. Relative to other Laravel projects, it's clear that Lumen has taken a backseat. This is evident by the documentation often referring to Laravel and the release cycle falling weeks after Laravel releases. In fact, since version 5, there have been only [10 Lumen releases](https://github.com/laravel/lumen/releases) compared to [132 for Laravel](https://github.com/laravel/framework/releases).


## Not so fast
Finally, one of the largest proponents for using Lumen is its performance. It still beats most other frameworks (Laravel included). However, Laravel is speeding up. We see from [Taylor's recent benchmarks](https://medium.com/@taylorotwell/benchmarking-laravel-symfony-zend-2c01c2b270f8) Laravel (without sessions) pushes 600 req/sec. This is still a third of what Lumen touts - around 1900 req/sec.

Nonetheless, I'm sure with additional optimizations one could improve Laravel's performance if 600 req/sec was preventing them from converting from Lumen. In addition, I wouldn't be surprised to see a few configuration options added to Laravel to facilitate *Lumen-like* performance boosts.

For these reasons I say *Lumen is dead*. But I believe Lumen will live on - just not as a separate framework. Instead, I expect Lumen will be rolled back into a future version of Laravel.

_**Need to convert Lumen to Laravel?** The [Lumen to Laravel Shift](https://laravelshift.com/convert-lumen-to-laravel) is now available to help you convert your Lumen projects to their Laravel equivalents._
