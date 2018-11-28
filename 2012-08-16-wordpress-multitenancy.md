---
title: "WordPress Multitenancy"
heading: "WordPress Multitenancy"
author: Jason McCreary
excerpt: "WordPress Multitenancy - a simple solution to a complex problem. This post covers creating a multitenant architecture for WordPress. Aimed at improving scalability and maintainability."
subheading: "WordPress Multitenancy - a simple solution to a complex problem. This post covers creating a multitenant architecture for WordPress. Aimed at improving scalability and maintainability."
layout: post
comments: true
permalink: /2012/08/wordpress-multitenancy/
categories:
  - Main Thread
tags:
  - development
  - wordpress
description: WordPress Multitenancy covers creating a multitenant architecture for WordPress. Aimed at improving scalability and maintainability.
---
***Update:** I updated this [WordPress multitenant install](http://jason.pureconcepts.net/2013/04/updated-wordpress-multitenancy/ "Updated: WordPress Multitenancy"). While this post still serves as a foundational reference, you should use the new solution.*

I got the opportunity to explore WordPress multitenancy for a recent consulting project. Multitenancy is something I have been interested in for months. Surprisingly, searching for *WordPress Multitenancy* returned no results. For those of you that aren't familiar, I'll provide a definition of multitenancy. For a detailed explination, I suggest reading [Multi-tenancy Explained][1].

> Multitenancy is an architecture in which a single instance of a software application serves multiple customers.

For WordPress, multitenancy means multiple WordPress sites running off a single WordPress codebase. This sounds similar to WordPress Multisite. However, WordPress Multisite combines all site resources (themes, plugins, etc) together under a single WordPress install (code and database). Whereas, multitenancy allows for completely individual WordPress sites (own resources and database) sharing a single WordPress codebase.

Any admin of a WordPress Multisite install bears the burden of the *Network Admin*. Any developer of multiple WordPress sites bears the burden of keeping WordPress updated. With WordPress multitenancy, the user has their own site and the developer manages WordPress in a single place. *One ring to rule them all* kind of thing. Adopting a multitenant architecture provides additional benefits, aside from management, such as scaling.

Let me be clear though, multitenancy is not a solution to a problem *with* WordPress. For most, WordPress Multisite and the integrated WordPress Updates cover the issues I mentioned. Multitenancy is a solution to an architectural problem. It's likely you found this post because you already know about multitenancy. So you're ready to setup WordPress multitenancy and I'm likely wasting keystrokes with this disclaimer.

## WordPress Setup

In Thomas Edison fashion, I'll walk through the failed attempts before the solution. First, let me outline some terminology and the initial architecture. I'm using the term *core* for the central WordPress codebase and *tenant* for an individual WordPress site. To start, I deployed WordPress (version 3.4.1) in its own directory. For good measure, this directory was not web accessible and I removed its **wp-content** directory. I also created two sites. Each with their own web accessible directories containing only the **wp-content** directory.

## Attempt #1: Symlinks

I symlinked all of the top-level WordPress files and directories in the tenant to the core.

White screen.

After reviewing the error logs, I noticed WordPress failed to include **wp-config.php**. WordPress performs directory traversal using PHP's `__FILE__` constant. Turns out, PHP resolves the path (converting all symlinks) before setting `__FILE__`. So WordPress looked for **wp-config.php** in the core directory. I need WordPress to look in the tenant directory.

Fail.

## Attempt #2: Hard Links

After *Googling,* I learned about hard links. Honestly, I still do not fully understand hard links. So I won't try to explain. The gist is hard links work like symlinks, but maintain a *hard* reference to the linked file. Essentially acting like a *physical* file exists. As such, hard links set the `__FILE__` constant with the tenant path instead of the core path.

But this introduced new problems. First, you can't hard link directories. There are workarounds. However they seemed platform specific. WordPress is compatible with a wide range of platforms. I did not want a solution that narrowed compatibility. Which leads to the second problem. I would have to hard link every core file of WordPress and re-create its directory structure.

Fail.

## Attempt #3: Modify WordPress

This was never *really* an option. Nonetheless, code to include **wp-config.php** was only in six files. I entertained this idea for 3 seconds. Then remembered anytime I updated WordPress requires an audit of the codebase. A never-ending battle and exactly why you don't modify core code.

Super Fail.

## Learning from Failure

Let's reflect… Symlimks worked – but PHP resolved paths preventing core from including the tenant code. Hard links were an option – but being limited to files make it a deployment nightmare. And, well, modifying WordPress is never a good option.

Nonetheless, like Mr. Edison, I learned something from my failures – 3 ways that don't work. I needed a solution with the ease of symlimks but the ability to configure core to reference the tenant. Enter **wp-config.php**.

## A WordPress Multitenancy Solution

The purpose of the **wp-config.php** is to configure WordPress. So leveraging it seemed appropriate. As learned from my attempts, the code that used `__FILE__` only failed when referencing **wp-config.php**. Theoretically other core code using `__FILE__` should not fail since it relates to other core files. In the end, the only references to the tenant are the **wp-content** directory and **wp-config.php**.

The **wp-content** directory is easy to configure as WordPress already provides a constant. [`WP_CONTENT_DIR`][2] allows you to modify the location of the **wp-content** directory. The fact that I can modify this inside of **wp-config.php** lent even more credence to this solution.

So I have come full circle. Linking the tenant **wp-config.php** is the key. If I make the assumption that core is used for a multitenant architecture (which is a safe assumption) things became easier. All I need to do is create a **wp-config.php** in core to serve as a placeholder. Then the core **wp-config.php** can include the tenant **wp-config.php**.

To determine the location of the tenant I use PHP's super global `$_SERVER['DOCUMENT_ROOT']`. Some developers advocate against trusting `$_SERVER` values. While I typically agree, this particular value is set with your server's configuration (e.g. `DocumentRoot` in Apache). Furthermore WordPress uses this value as well. So I haven't *introduced* a security risk.

By solving the issue of including the tenant **wp-config.php**, I am free to symlink the rest of the core files and directories.

### WordPress Multitenancy Architecture Diagram

<img title="Wordpress  Multitenancy Architecture Diagram" src="/images/wordpress-multitenancy-architecture-diagram.png" alt="Wordpress  Multitenancy Architecture Diagram" width="423" height="410" />

### Core wp-config.php

    <?php
    /**
     * The base configurations of the WordPress.
     *
     * ...
     *
     * @package WordPress
     */
    // NOTE: this WordPress install is configured for multitenancy
    require_once dirname($_SERVER['DOCUMENT_ROOT']) . '/wp-config.php';
    
    /* That's all, stop editing! Happy blogging. */
    
    /** Absolute path to the WordPress directory. */
    if(!defined('ABSPATH'))
        define('ABSPATH', dirname(__FILE__) . '/');
    
    /** Sets up WordPress vars and included files. */
    require_once(ABSPATH . 'wp-settings.php');
    

### Tenant wp-config.php

    <?php
    /**
     * The base configurations of the WordPress.
     *
     * ...
     *
     * @package WordPress
     */
    
    // NOTE: file lives outside webroot for additional security
    
    // modify the config file based on environment
    if (strpos($_SERVER['HTTP_HOST'], 'local') !== false) {
        $config_file = 'config/wp-config.dev.php';
    }
    else {
        $config_file = 'config/wp-config.prod.php';
    }
    
    $path = dirname(__FILE__) . '/';
    require_once $path . $config_file;
    
    /** Database Charset to use in creating database tables. */
    define('DB_CHARSET', 'utf8');
    
    /** The Database Collate type. Don't change this if in doubt. */
    define('DB_COLLATE', '');
    
    /**#@-*/
    
    /**#@+
     * Authentication Unique Keys and Salts.
     *
     * ...
    

If you are curious lines 13-18, I suggest you read my post on [Configuring WordPress for Multiple Environments][3] or [watch my talk][4] from WordCamp Chicago 2011.

## More Testing

While I tested and am currently running a multitenant architecture for WordPress sites in production, this solution needs more testing. It's almost too simple. Which leads me to believe this has been considered by WordPress. You think <a href=&rdquo;http://wordpress.com&rdquo; title=&rdquo;WordPress.com&rdquo;>WordPress.com</a> installs WordPress for each of the millions of sites they host? Doubt it…

Nonetheless, WordPress is a large, feature-rich piece of software with a huge ecosystem of over 20,000 plugins and uncounted themes. If you adopt the solution or have one of your own please comment with your feedback and report your findings.

This post was inspired by an unconference talk on multitenancy given at [PHP|tek 12][5] and a post by [Mark Jaquith][6] on [WordPress Skeleton][7]. Also a shout out to [VIA Studio][8] for providing a testbed and adopting this WordPress multitenancy solution.

 [1]: http://vikashazrati.wordpress.com/2008/06/23/multi-tenancy-explained/ "Multi-tenancy Explained"
 [2]: http://codex.wordpress.org/Editing_wp-config.php#Moving_wp-content "Moving wp-content"
 [3]: http://viastudio.com/2011/02/08/configuring-wordpress-multiple-environments/ "Configuring WordPress for Multiple Environments"
 [4]: http://wordpress.tv/2012/01/24/jason-mccreary-configuring-wordpress-for-multiple-environments/ "Jason McCreary – Configuring WordPress for Multiple Environments"
 [5]: http://tek.phparch.com "PHP|tek Conference"
 [6]: http://markjaquith.com "Mark Jaquith"
 [7]: http://markjaquith.wordpress.com/2012/05/26/wordpress-skeleton/ "WordPress Skeleton"
 [8]: http://viastudio.com "VIA Studio"
