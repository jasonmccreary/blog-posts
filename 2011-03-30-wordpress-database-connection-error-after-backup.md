---
title: WordPress Database Connection Error After Migration
author: Jason McCreary
excerpt: 'Sharing my *moment* with WordPress and its dreaded &quot;Error establishing a database connection&quot; after updating the database.'
layout: post
comments: true
permalink: /2011/03/wordpress-database-connection-error-after-backup/
categories:
  - Main Thread
tags:
  - development
  - rant
  - wordpress
description: Sharing my *moment* with WordPress and its dreaded &quot;Error establishing a database connection&quot; after updating the database.
keywords: wordpress, error, database connection, multisite
---
I was migrating a WordPress database from Production to Staging the other day and received the dreaded *"Error establishing a database connection"* afterwards. I verified the database name, username, password, host, etc. Everything was correct. What the hell?

Then it hit me, we enabled multisite for this WordPress install. After looking under the hood, although WordPress could connect to the database, it didn&rsquo;t find a matching entry for our staging URL. Apparently it still displays *"Error establishing a database connection"* message. A quick update to the records in the `wp_blogs`, `wp_sites`, and `wp_options` database tables resolved this.

Hope that helps someone. Check out my other post if you&rsquo;re interested in [setting up WordPress for multiple environments][1].

 [1]: http://viastudio.com/2011/02/08/configuring-wordpress-multiple-environments/
