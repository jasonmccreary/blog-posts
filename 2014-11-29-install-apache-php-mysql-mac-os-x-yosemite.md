---
title: "Installing Apache, PHP, and MySQL on Mac OS X Yosemite"
heading: "Installing Apache, PHP, and MySQL on Mac OS X Yosemite"
author: Jason McCreary
excerpt: "This is an update for Mac OS X Yosemite of a previous post on installing Apache, PHP, and MySQL for Mac OS X."
subheading: "This is an update for Mac OS X Yosemite of a previous post on installing Apache, PHP, and MySQL for Mac OS X."
layout: post
comments: true
categories:
  - Main Thread
tags:
  - apache
  - development
  - mysql
  - php
description: "This is an update for Mac OS X Yosemite of a previous post on installing Apache, PHP, and MySQL for Mac OS X."
subheading: "This is an update for Mac OS X Yosemite of a previous post on installing Apache, PHP, and MySQL for Mac OS X."
---
***OS X El Capitan Update:** While these instructions still work, I wrote a new post for [installing Apache, PHP, and MySQL on Mac OS X El Capitan](/2015/10/install-apache-php-mysql-mac-os-x-el-capitan/).*

I recently upgraded to Mac OS X Yosemite. It seems OS X Yosemite makes my original post on [installing Apache, PHP, and MySQL on Mac OS X](/2012/10/install-apache-php-mysql-mac-os-x/) obsolete. Specifically, Yosemite includes Apache 2.4. This post is a complete update for installing Apache, PHP, and MySQL on Mac OS X Yosemite.

A reminder that Mac OS X runs atop UNIX. So most UNIX software installs easily on Mac OS X. Furthermore, Apache and PHP come packaged with OS X. To create a local web server, all you need to do is enable them and *install* MySQL.

I am aware of the web server software available for Mac OS X, notably [MAMP][1]. These get you started quickly. But they forego the learning experience and, as most developers report, can become difficult to manage.

## Getting Started

First, open the *Terminal* app and switch to the `root` user to avoid permission issues while running these commands.

    sudo su -

## Enable Apache on Mac OS X

    apachectl start

Verify *It works!* by accessing <http://localhost>

## Enable PHP for Apache
First, make a backup of the default Apache configuration. This is good practice and serves as a comparison against future versions of Mac OS X.

    cd /etc/apache2/
    cp httpd.conf httpd.conf.bak

Now edit the Apache configuration. Feel free to use *TextEdit* if you are not familiar with *vi*.

    vi httpd.conf

Uncomment the following line (remove `#`):

    LoadModule php5_module libexec/apache2/libphp5.so

Restart Apache:

    apachectl restart

You can verify PHP is enabled by creating a [`phpinfo()`](http://php.net/manual/en/function.phpinfo.php) page in your `DocumentRoot`.

The default `DocumentRoot` for Mac OS X Yosemite is `/Library/WebServer/Documents`. You can verify this from your Apache configuration.

    grep DocumentRoot httpd.conf

Now create the `phpinfo()` page in your `DocumentRoot`:

    echo '<?php phpinfo();' > /Library/WebServer/Documents/phpinfo.php

Verify PHP by accessing <http://localhost/phpinfo.php>

## Install MySQL on Mac OS X

**Note**: If you are upgrading MySQL you should skip this section and instead [read this](http://coolestguidesontheplanet.com/upgrade-mysql-database-5-5-5-6-osx-10-8-mountan-lion/).

1.  [Download][2] the MySQL DMG for Mac OS X
2.  Install *MySQL*

The **README** suggests creating aliases for `mysql` and `mysqladmin`. However there are other commands that are helpful such as `mysqldump`. Instead, I [updated my path](http://superuser.com/questions/69130/where-does-path-get-set-in-os-x-10-6-snow-leopard) to include `/usr/local/mysql/bin`.

    export PATH=/usr/local/mysql/bin:$PATH

**Note**: You will need to open a new *Terminal* window or run the command above for your path to update.

I also run `mysql_secure_installation`. While this isn&rsquo;t necessary, it&rsquo;s good practice.

## Connect PHP and MySQL
You need to ensure PHP and MySQL can communicate with one another. There are [several options][3] to do so. I do the following:

    cd /var 
    mkdir mysql
    cd mysql
    ln -s /tmp/mysql.sock mysql.sock

## Additional Configuration (optional)
The default configuration for Apache 2.4 on OS X Yosemite seemed pretty lean. For example, common modules like `mod_rewrite` were disabled. You may consider enabling this now to avoid forgetting they are disabled in the future.

I edited my Apache Configration:

    vi /etc/apache2/httpd.conf

I uncommented the following lines (remove `#`):

    LoadModule deflate_module libexec/apache2/mod_deflate.so
    LoadModule expires_module libexec/apache2/mod_expires.so
    LoadModule rewrite_module libexec/apache2/mod_rewrite.so

**Note**: Previous version of Mac OS X ran Apache 2.2. If you upgraded OS X and previously configured Apache, you may want to read more about [upgrading to to Apache 2.4 from Apache 2.2](http://httpd.apache.org/docs/trunk/upgrading.html).

If you develop multiple projects and would like each to have a unique url, you can [configure Apache *VirtualHosts* for Mac OS X](/2014/11/configure-apache-virtualhost-mac-os-x/).

If you would like to install [PHPMyAdmin][4], return to my original post on [installing Apache, PHP, and MySQL on Mac OS X](/2012/10/install-apache-php-mysql-mac-os-x/).

 [1]: http://www.mamp.info/en/index.html "MAMP"
 [2]: http://dev.mysql.com/downloads/mysql/
 [3]: http://stackoverflow.com/questions/4219970/warning-mysql-connect-2002-no-such-file-or-directory-trying-to-connect-vi
 [4]: http://www.phpmyadmin.net/ "PHPMyAdmin"
