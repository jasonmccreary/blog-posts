---
layout: post
title: "Benchmark: Octopress vs. WordPress"
heading: "Benchmark: Octopress vs. WordPress"
date: 2013-01-28 14:50
comments: true
categories: 
  - Main Thread
excerpt: "A follow-up post reviewing the benchmarks performed during my recent migration from WordPress to Octopress."
subheading: "A follow-up post reviewing the benchmarks performed during my recent migration from WordPress to Octopress."
description: "A follow-up post reviewing the benchmarks performed during my recent migration from WordPress to Octopress."
subheading: "A follow-up post reviewing the benchmarks performed during my recent migration from WordPress to Octopress."
---
I recently [migrated from WordPress to Octopress](http://jason.pureconcepts.net/2013/01/migrating-wordpress-octopress/ "Migrating from WordPress to Octopress"). During the migration I benchmarked the performance between WordPress and Octopress.

## Server Configuation
Last summer I [migrated WordPress to Amazon EC2](http://jason.pureconcepts.net/2012/07/migrating-wordpress-to-amazon-ec2/ "Migrating WordPress to EC2"). I decided to stay on EC2 for Octopress for two reasons. First, a [micro instance](http://aws.amazon.com/ec2/instance-types/ "EC2 Micro Instance") is, essentially, free. Second, if the micro instance can serve a WordPress blog, it can serve a static HTML blog. 

I made a few optimizations to the Apache configuration. Namely decreased `Timeout`, `KeepAlive` enabled, and tweaked connection limits. I also disabled `AllowOverride`.

For PHP I installed and enabled [APC](http://php.net/manual/en/book.apc.php "Alternative PHP Cache").

WordPress used [object-cache](http://wordpress.org/extend/plugins/apc/ "APC Object Cache Backend") and [Hyper Cache](http://wordpress.org/extend/plugins/hyper-cache/ "Hyper Cache").

## Benchmark Configurations
I benchmarked the following configurations using the [Apache benchmarking tool](http://httpd.apache.org/docs/2.2/programs/ab.html "Apache benchmarking tool") (`ab`):

1. WordPress Page Request
2. WordPress Post Request
3. Octopress Page Request
4. Octopress Post Request

Each benchmark made 1000 requests for 50, 100, 150, and 200 concurrent connections using both `close` and `keep-alive`. I performed each benchmark 3 times to average the result.

## Benchmark Results
I created two graphs from the benchmark results:

- Average request per second
- Average time per request (in milliseconds).

### Request Per Second
<figure>
  <img src="/images/benchmark-request-per-second.png" alt="Graph: Requests Per Second" title="Graph: Requests Per Second" />
  <figcaption>Requests Per Second (<code>Connection: close</code>)</figcaption>
</figure>

<figure>
  <img src="/images/benchmark-request-per-second-keep-alive.png" alt="Graph: Requests Per Second (keep-alive)" title="Graph: Requests Per Second (keep-alive)" />
  <figcaption>Requests Per Second (<code>Connection: keep-alive</code>)</figcaption>
</figure>

Without surprise, Octopress is faster than WordPress. Roughly 3 times faster (300%). In some cases reaching an impressive 2,000 requests per second for `close` connections, and nearly 3,000 requests per second for `keep-alive` connections.

Octopress performed the same as WordPress for page requests with 200 concurrent `close` connections. However, this implicates the micro instance more than Octopress or WordPress.  

### Time Per Request
<figure>
  <img src="/images/benchmark-time-per-request.png" alt="Graph: Time Per Request" title="Graph: Time Per Request" />
  <figcaption>Time Per Request (<code>Connection: close</code>)</figcaption>
</figure>

<figure>
  <img src="/images/benchmark-time-per-request-keep-alive.png" alt="Graph: Time Per Request (keep-alive)" title="Graph: Time Per Request (keep-alive)" />
  <figcaption>Time Per Request (<code>Connection: keep-alive</code>)</figcaption>
</figure>

The time per request results are similar to the requests per second results. Even the page request anomaly appeared. Nevertheless, Octopress is still faster than WordPress.

For most of the benchmarks, Octopress response times were under 100 milliseconds. This not only improves user experience, but search engine optimization as well.

### Sever Stats
The following are screenshots of memory and CPU usage graphs taken from my [New Relic](http://newrelic.com "New Relic") dashboard during the benchmarks.

<figure>
  <img src="/images/wordpress-server-stats.png" alt="Server Stats: WordPress" title="Server Stats: WordPress" />
  <figcaption>Server Stats: WordPress</figcaption>
</figure>

With WordPress, the server spikes well above its physically memory and CPU limits. 

<figure>
  <img src="/images/octopress-server-stats.png" alt="Server Stats: Octopress" title="Server Stats: Octopress" />
  <figcaption>Server Stats: Octopress</figcaption>
</figure>

With Octopress, the server has 60% of its physical memory available and `vi` shows up in the *Top 5* processes &mdash; wow.

## Benchmark Conclusion
It's no surprise Octopress is faster than WordPress. Octopress uses static files whereas WordPress uses 10,000 lines of PHP code performing various database queries. While [you can make WordPress faster](http://jason.pureconcepts.net/2012/08/21-ways-wordpress-fast/ "21 ways to make WordPress fast"), it's worth nothing that without caching WordPress did not finish any of the benchmarks above. In fact, it crashed my server.

I plan to add another benchmark configuration for [nginx](http://wiki.nginx.org/Main) and possibly [Varnish](https://www.varnish-cache.org/). In addition, I'm monitoring the potential search engine optimizations from this migration. Look for posts on both in the coming months. In the meantime, I welcome your feedback.
