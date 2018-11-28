---
title: "21 Ways to Make WordPress Fast"
heading: "21 Ways to Make WordPress Fast"
author: Jason McCreary
excerpt: "WordPress is slow. We know. It's on you to make WordPress fast. This post offers 21 ways to make your site and WordPress faster."
subheading: "WordPress is slow. We know. It's on you to make WordPress fast. This post offers 21 ways to make your site and WordPress faster."
layout: post
comments: true
permalink: /2012/08/21-ways-wordpress-fast/
categories:
  - Talks
description: WordPress is slow. We know. It's on you to make WordPress fast. This post offers 21 ways to make your site and WordPress faster.
---
This post is from my talk at WordCamp Chicago and WordCamp Louisville – *21 Ways to Make WordPress Fast*. Slides are on [SlideShare][1]. Video from WordCamp Chicago has been posted on [WordPress.tv][2]. WordPress is slow. As WordPress powers over 55 million sites, it is literally slowing down the web. I'm offering 21 ways to make your WordPress site faster. Implementing any will improve your site performance, thus doing your part in speeding up the web.

## What do you mean *fast*?

A page has many parts. Server-code and client-code. Dynamic content and static content. Simply put, we want all these to *load* fast. But each has a certain *weight*. In the case of performance, weight is a factor of quantity and size. Weight and performance have an inverse relationship. Therefore, if we decrease the weight, we increase performance.

## Making WordPress Fast

WordPress itself is pretty *heavy*. On each request, thousands of lines of code are loaded executing dozens of database queries. All just to *generate* the page. While a small part in the process of *loading* a page, WordPress can do this faster. But at the end of the day, making WordPress fast is also an exercise in making your site faster. So several of these tips apply beyond WordPress.

### Valid Code

Invalid code requires the browser to make assumptions about your page. While ultimately this could result in incorrect rendering, it also slows down rendering. Furthermore, invalid code can lead to front-end development headaches. It's simple to validate your code using the [W3C Online Validator][3]. Do it.

### Permalink Settings

WordPress offers Permalinks to customize your site's URLs structure. Internally WordPress uses this structure to process your request. Some structures can make this job difficult – decreasing performance. Generally speaking, the more WordPress can match against your structure the better. 

Quick tests showed barely a 1% decrease in a permalink structure of  
`/%postname%/` vs. `/%year%/%monthnum%/%postname%/`. This may be a greater concern in previous versions of WordPress. In the end, consider search engine optimization over performance.

### Use Less Plugins

There are over 20,000 WordPress Plugins. The ease at which you can install WordPress Plugins is both a beauty and a curse. Often leading to *plugin bloat*. Each plugin adds more and more code for WordPress to load. Adding more weight and decreasing performance..

It comes down to quantity and quality. Instead of using 3 social sharing plugins, use 1. Plugins may also be developed poorly. Using hooks like `init` inappropriately. You should audit your plugins often and deactivate/remove those that are useless.

### 404s, Trackbacks, and Pingbacks

A fast page makes as few requests as possible. Google, for example, makes around 8. 404s are wasted requests. Your page should not have 404s.

Trackback and Pingbacks are notifications between websites. While often background requests, they still use resources and add traffic. In addition, it can lead to SPAM. For these reasons combined, I disable it. This can be done in *Settings → Discussion*.

### Reading Settings

Ensure your per page settings are reasonable. A small value leads to more pagination (and therefore more page requests) and a large value leads to larger pages. Also consider displaying excerpts instead of full content. You can adjust these in *Settings → Reading*.

### Move CSS to the top, JavaScript to the bottom

Per the HTML specification CSS should be loaded in the `<head>` section. Linking stylesheets outside `<head>` will block progressive loading. This prevents the browser from displaying content as it is loaded.

JavaScript also *blocks* progressive loading. When a `<script>` tag is encountered the browser interprets this code before loading more of the page. Moving your `<script>` to the bottom (footer) allows a majority of the page to load first.

While the page still has the same *weight*, these simple adjustments make the page *appear* faster. However, depending on your UI, this may lead to the dreaded [flash of unstyled content][4].

### Content Delivery Networks

A Content Delivery Network (CDN) distributes your resources across the globe. This results in faster response times for the user. Ultimately making your site load faster. There are additional benefits to CDNs such as increased parallel downloads and redundancy.

### Multiple/Static Domains

Most browsers download 2 resources in parallel per domain. Since most resources are on a single domain (yours) the browser queues requests. By using multiple domains, the browser can download more resources at a time. However, there is a balance. Most suggest using 2-4 separate domains to be best.

In addition, using a separate domain for *static* content (images, CSS, etc) will prevent unnecessary data – such as a cookie – to be sent for each request.

### Sharing Widgets

Sharing Widgets often include their own JavaScript and CSS within an iFrame. Likely loading resources from an external domain. All of which go against. Understand how the sharing widget works so you can implement it in a way that doesn't slow down your page.

### Comments and Gravatar

Similar to Sharing Widgets, Gravatar can add significant weight to your page. Each comment includes a Gravatar and therefore an additional request and image resource.

If Gravatar is enabled, the most waste comes from commenters without a Gravatar. Using *Blank* vs. *Mystery Man* is negligible. Although *Blank* was indeed faster, the aesthetics of *Mystery Man* likely outweigh any gain. Disabling *Avatar Display* (*Settings → Discussion*), decreased load times by 10%.

### Use CSS Image Sprites

Using [CSS Image Sprites][5] help decrease the number of image requests your page makes. Since most pages contain many images, this can greatly reduce the total number of requests. In addition, the file size of a single, albeit larger image is less than the sum of the original images.

Creating a CSS image sprite and coding its styles can be time consuming. But once you've mastered this skill, I guarantee your sites will be more performant.

    #header-logo {
      background: url(../images/some-sprite.png) no-repeat -119px -9px;
      height: 35px;
      width: 111px;
    }
    

### Minification

Similar to compression, minification is another way to reduce file size. Minification removes unnecessary characters, such as whitespace and comments. I also consider condensing all files into a single file part of *minification*.

For example, CSS files are often split out for ease of development. This is unnecessary in production. Condensing them not only reduces file size, but also the number of requests.

### Compression

Compressing your resources can greatly reduce the amount of data transferred between your server and the client. Services like [Smush.it][6] can compress image resources. Using gzip compression for text based resources (HTML, CSS, JavaScript) can reduce sizes by 70%.

Enabling gzip compression in Apache is available through [mod_deflate][7]. I use the following configuration for this site:

    AddOutputFilterByType DEFLATE text/html text/plain text/xml text/css text/javascript application/javascript application/x-javascript
    

### Resource Caching

Most site resources are static. Your site's background images, CSS, and JavaScript don't change frequently. As such, these resources should be cached. Caching tells the browser to save these resources instead of requesting them again on subsequent page requests.

Adding an `Expires` header by file type is simple in Apache:

    ExpiresActive On
    ExpiresByType image/gif "access plus 6 months"
    ExpiresByType image/jpeg "access plus 6 months"
    ExpiresByType image/png "access plus 6 months"
    ExpiresByType text/css "access plus 6 months"
    ExpiresByType text/javascript "access plus 6 months"
    ExpiresByType application/javascript "access plus 6 months"
    ExpiresByType application/x-javascript "access plus 6 months"

In addition, I remove `ETags`. Unless you understand their role in caching, removing them is generally best.

    Header unset ETag
    FileETag None

### WordPress Caching

As mentioned earlier, WordPress is heavy. Thousands of lines of code are loaded and dozens of database queries are executed on every request. Even for highly-dynamic website, content is mostly static. As such, it can be cached.

There are several WordPress plugins. Some are basic, like [Hyper-Cache][8], only caching page content. Others are highly configurable and cache additional WordPress resources, like [W3 Total Cache][9]. Ultimately the less WordPress code loaded the better, ideally avoiding it entirely.

### PHP Caching

PHP Caching is important for when WordPress does run. While hopefully this isn't often if you've implemented WordPress Caching, it improves performance nonetheless. As PHP is an interpreted language, code is typically parsed and run on each request. An opcode cache saves the parsed PHP code. APC is the most common PHP cache. If APC is available, you should install [APC Object Cache][10] or enable this with W3 Total Cache.

### Database Caching

While most of the plugins above include Database Caching, I have included it for completeness. WordPress executes dozens of queries on each request. Database connectivity is one of the more expensive operations.

### Extra Data

Over time, WordPress can store a lot of *extra* data. This includes, revision data, trashed data, and custom meta data.

By default, WordPress tracks revisions for pages and posts. If you have a large or an old site, records for revisions can outgrow records for actual content. While disabling revisions might be too extreme, you can set a [maximum number of revisions][11] as well as the [auto save interval][12] in your **wp-config.php** file.

Similar to revisions, trashed items are just taking up space in the database. Be sure to empty the trash regularly. You can also set the frequency with the [`EMPTY_TRASH_DAYS`][13] configuration setting.

Be mindful when using custom fields. The database structure for custom fields is one to many. Using custom fields can quickly lead to an N+1 Problem. If you have data that you require for all posts, see if a [custom post type][14] or plugin solves the problem.

### Database Optimizations

The database engine used by MySQL can lend a slight performance boast. This depends heavily on your MySQL version. Prior to 5.5, using MyISAM may provide better read performance. Whatever the database engine, most WordPress sites can be tweaked for heavier read operations.

### Redirects

Avoid redirects. Especially at the WordPress level. If you must perform redirects, do them at the Apache level. Add a `RewriteRule` or preferably a `Redirect`.

### Apache Configuration

There are a few performance gains at the Apache level. Each of which squeeze a few requests per second. Your ability to implement these will depend greatly server access.

Avoiding [mod_rewrite][15]. By default, WordPress uses `RewriteRule` to route requests to **index.php**.

    RewriteRule ^index\.php$ - [L]
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteCond %{REQUEST_FILENAME} !-d
    RewriteRule . /index.php [L]
    

Since Apache 2.2.17, `FallBackResource` performs the same:

    FallbackResource /index.php
    

When possible, add these directly to your site's Apache configuration file. Doing so allows them to load with Apache and not for every page request (when using .htaccess). Might add a few requests per second depending on your setup.

### Hosting

You'll be lucky to get more than 10 requests/second on $3/month shared hosting. Hosting on a WordPress optimized host or VPS will immediately improve this. Upgrading your hosting is likely the simplest way to make WordPress fast. Albeit costlier. 

## Tools and Resources

I find the following tools helpful when benchmarking performance:

*   [Firebug][16] with [YSlow!][17]
*   [siege][18] and [ab][19]

I also recommend the following resources:

*   [Yahoo! Performance Rules][20]
*   [Google PageSpeed][21]
*   [High Performance Web Sites][22] and [Even Faster Web Sites][23]

Go forth and make WordPress fast…

 [1]: http://www.slideshare.net/mccreaja/21-ways-to-make-wordpress "Slides on SlideShare"
 [2]: http://wordpress.tv/2012/10/04/jason-mccreary-21-ways-to-make-wordpress-fast/
 [3]: http://validator.w3.org
 [4]: https://stevesouders.com/hpws/css-fouc.php
 [5]: http://css-tricks.com/css-sprites/
 [6]: http://www.smushit.com/ysmush.it/
 [7]: http://httpd.apache.org/docs/2.2/mod/mod_deflate.html
 [8]: http://wordpress.org/extend/plugins/hyper-cache/
 [9]: http://wordpress.org/extend/plugins/w3-total-cache/
 [10]: http://wordpress.org/extend/plugins/apc/
 [11]: http://codex.wordpress.org/Editing_wp-config.php#Specify_the_Number_of_Post_Revisions
 [12]: http://codex.wordpress.org/Editing_wp-config.php#Modify_AutoSave_Interval
 [13]: http://codex.wordpress.org/Editing_wp-config.php#Empty_Trash
 [14]: http://codex.wordpress.org/Post_Types#Custom_Types
 [15]: http://httpd.apache.org/docs/2.2/mod/mod_rewrite.html
 [16]: http://getfirebug.com
 [17]: http://developer.yahoo.com/yslow/
 [18]: http://www.joedog.org/siege-home/
 [19]: http://httpd.apache.org/docs/2.2/programs/ab.html
 [20]: http://developer.yahoo.com/performance/rules.html
 [21]: https://developers.google.com/speed/pagespeed/
 [22]: http://www.amazon.com/High-Performance-Web-Sites-Essential/dp/0596529309/ref=pd_bxgy_b_text_y
 [23]: http://www.amazon.com/Even-Faster-Web-Sites-Performance/dp/0596522304
