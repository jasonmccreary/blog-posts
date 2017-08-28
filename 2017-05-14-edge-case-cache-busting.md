---
layout: post
title: 'An edge case for cache busting'
date: 2017-05-14 11:37
comments: true
categories:
  - Main Thread
description: 'Most high performance web sites optimize for the sad path. In this article, we address a way to optimize for the happy path too.'
subheading: 'Most high performance web sites optimize for the sad path. In this article, we address a way to optimize for the happy path too.'
excerpt: 'Most high performance web sites optimize for the sad path. In this article, we address a way to optimize for the happy path too.'
---
Let's say we're architecting a high-performance website. We know from [Steve Sounders' books](https://stevesouders.com/) we see the most performance gains by focusing on frontend optimizations.

To start improving performance, we may do the following:

- **Concatenate and minify assets.** By condensing all of our JavaScript and CSS into a single file (respectively) we decrease network traffic. It's also faster to download a single larger file than downloading several smaller files.

- **Serve content from the edge**. By serving content from a server that is physically closer to the user we improve performance. We can use a content delivery network (CDN) to do so.

- **Set cache and compression headers**. Since these assets do not change often only want the user to download them once. We can do so by setting the [expiration headers](https://developers.google.com/speed/docs/insights/LeverageBrowserCaching) to be far in the future (say one year). In addition, we can decrease the download size by [compressing them](https://developers.google.com/speed/docs/insights/EnableCompression).

Nowadays, this architecture is pretty easy to implement. Tools like [webpack](https://webpack.js.org/) or [gulp](http://gulpjs.com/) and services from [CloudFlare](https://www.cloudflare.com/) or [Amazon CloudFront](https://aws.amazon.com/cloudfront/) will handle most (if not all) of this for you.

However, this architecture has a known problem. Technically, anytime you implement browser caching you will encounter this problem. Let's take a closer look at this problem and a common solution.

## Busting the cache

> There are only two hard things in Computer Science: cache invalidation and naming things.

While true, invalidating the cache is not so hard in this case. Due to the nature of the web, we have a *centralized cache* rather than a *distributed cache*. When a user requests our web page, we have the opportunity to invalidate the cache and load new assets.

A common practice is to version file names or append a query string parameter. While you can do this manually, it's likely the tool you use to concatenate and minify your files can do this too. I recommend using checksum hashes as opposed to version numbers.

Now the next time a user requests our web page, the paths to the assets will be different causing them to be downloaded and cached.

## Maximizing cache hits

> Everybody has a plan until they get hit in the mouth

The primary goal of this architecture is for users to only download these assets once. Then, on subsequent visits, these assets would load from their local browser cache greatly improving performance.

This architecture achieves this goal. Yet it's only optimized for the *sad path*. That is when a user has an empty or stale cache. In doing, so we've actually degraded the performance of the *happy path*. That is when a user has a primed cache.

Sites with assets that don't change frequently or don't have high traffic may not notice this trade off. Hence the double entendre in the title of *edge case*. Nonetheless, I want to emphasize this trade off as similar articles rarely do.

Let's play through a user flow under this architecture:

1. User visits site for first time
2. User downloads assets
3. User visits site again
4. Browser loads assets from cache
5. Developer publishes new assets
6. User visits site again
7. User downloads assets

On the surface this seems good. The user downloaded the assets and utilized the cache upon a subsequent visit. Then when we updated the assets, the user downloaded the new assets the next time they visited the site.

The problem is with the last step. The user downloaded *all* the assets again. While these assets were indeed new, it's likely only a small amount of the file changed. As such, having a user with a primed cache download *everything* again is not optimal.

Let's use the condensed JavaScript file as an example. While custom JavaScript code may change frequently, most of the non-custom code will not. This

If we split our assets into two files we can optimize this architecture further while not adding many additional requests. So for the JavaScript file, we condense the infrequently changed code to one file and frequently changed code to another. We can do the same for our CSS.

Now if we play through the same user flow the last step becomes _User downloads only **changed** assets_. This is far more optimized. Especially for high traffic websites. If we consider separating out jQuery (40KB minimized) for a site with 1 million hits per month, that's 40GB of savings. Although that may not sound like much in the modern age of the internet, that could be the difference between plan tiers with your CDN.
