---
layout: post
title: "Updated: WordPress Multitenacy"
date: 2013-04-08 12:33
comments: true
categories: 
  - Main Thread
excerpt: An update to my solution for a multitenant WordPress install. With improvements in maintainability and extensibility. 
description: An update to my solution for a multitenant WordPress install. With improvements in maintainability and extensibility.
---
Although I [migrated this blog to Octopress](http://jason.pureconcepts.net/2013/01/migrating-wordpress-octopress/ "Migrate WordPress to Octopress"), I still develop with WordPress. A few months ago I shared a solution for [WordPress multitenancy](http://jason.pureconcepts.net/2012/08/wordpress-multitenancy/ "WordPress Multitenancy"). This post received great feedback. I incorporated several suggestions into an updated WordPress multitenant install.

## A single WordPress symlink
My original solution symlinked all-the-things. The entire top-level WordPress structure symlinked to the core WordPress install. While this works, it's brittle. If the top-level WordPress structure changed in a new version the tenant install may break. Although I mitigated this with an install script, there was room for improvement.

*Bastiaan* pointed out that you could [move WordPress into its own directory](http://codex.wordpress.org/Giving_WordPress_Its_Own_Directory). After following the steps outlined in the WordPress Codex (carefully in order) you can use a single symlink. A much cleaner solution.

Having a single symlink also makes maintaining tenant installs easier. I can quietly install a new version of WordPress while tenant sites safely point to an old. Then update their symlink (thus updating WordPress) as needed.

## WordPress Must Use Plugins
WordPress also introduced [Must Use Plugins](http://codex.wordpress.org/Must_Use_Plugins). A WordPress install *must use* these plugins - meaning they are automatically activated, and can not be deactivated. Using `WPMU_PLUGIN_DIR` and `WPMU_PLUGIN_URL` I can configure the location of `mu-plugins` just as I did for `wp-content`. Now the WordPress multitenant install can share even more between the tenants.

## A note about security
*Strebel* left a comment that there were *"significant security concerns"* with my WordPress multitenant solution. Unfortunately he did not elaborate. Thanks *Strebel*…

I am not a sysadmin. Nonetheless, to mitigate any security concerns I set permissions of the core WordPress directories and files to 755 and 644 respectively. In addition, all of the core WordPress are owned by a non-tenant, non-web user. **Note:** This will not work in an suPHP environment.

In addition, I have moved some of the configuration settings related to the multitenant install to the core `wp-config.php`. This prevents tenants from changing their configuration. And as you can not redeclare PHP contants, they can not overwrite this configuration.

``` php Core wp-config.php
// set global configurations
define('WP_CONTENT_DIR', $_SERVER['DOCUMENT_ROOT'] . '/wp-content');
define('WP_CONTENT_URL', 'http://' . $_SERVER['SERVER_NAME'] . '/wp-content');

// load site-specific configurations
require_once dirname($_SERVER['DOCUMENT_ROOT']) . '/wp-config.php';
```

I also follow common WordPress secuity practices, such moving `wp-config.php` outside webroot for both the core and tenant installs. 

## WordPress multitenant structure
The web directory of a tenant using the updated WordPress multitenant install:

	webroot$ ls -l
	total 16
	-rw-r--r--  1 jason  staff  200 Mar 30 13:35 .htaccess
	-rw-r--r--  1 jason  staff  405 Mar 30 16:17 index.php
	lrwxr-xr-x  1 jason  staff   20 Apr  6 14:30 wordpress -> /opt/wordpress/3.5.1
	drwxr-xr-x  6 jason  staff  204 Mar 30 16:35 wp-content

A few notes:

- Core WordPress files live in a versioned subdirectory of `/opt/wordpress/`. Such a structure allows for multiple WordPress installs.
- `wp-content` lives at the top-level.
- `mu-plugins` is a symlink under `wp-content`.

As always, I welcome your feedback.
