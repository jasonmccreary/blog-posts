---
title: "Installing Apache, PHP, and MySQL on Mac OS X Sierra"
heading: "Installing Apache, PHP, and MySQL on Mac OS X Sierra"
author: Jason McCreary
excerpt: "This is an update for Mac OS X Sierra of a previous post on installing Apache, PHP, and MySQL for Mac OS X."
subheading: "This is an update for Mac OS X Sierra of a previous post on installing Apache, PHP, and MySQL for Mac OS X."
layout: post
comments: true
categories:
  - Main Thread
tags:
  - apache
  - development
  - mysql
  - php
description: "This is an update for Mac OS X Sierra of a previous post on installing Apache, PHP, and MySQL for Mac OS X."
subheading: "This is an update for Mac OS X Sierra of a previous post on installing Apache, PHP, and MySQL for Mac OS X."
---
_**macOS Update:** While these instructions still work, there are new posts for recent versions of macOS, the latest being [Install Apache, PHP, and MySQL on macOS Mojave](/2018/11/install-apache-php-mysql-mac-os-x-mojave/)._

_**PHP Update:** Mac OS X Sierra comes pre-installed with PHP version 5.6, however the latest version of PHP is 7.1. After you complete this post, you should [upgrade PHP on Mac OS X](/2016/09/upgrade-php-mac-os-x/)._

_**Note:** This post is for new installations. If you have [installed Apache, PHP, and MySQL for Mac OS El Capitan](/2015/10/install-apache-php-mysql-mac-os-x-el-capitan/), read my post on [Updating Apache, PHP, and MySQL for Mac OS X Sierra](/2016/09/update-apache-php-mysql-mac-os-x-sierra/)._

Mac OS X runs atop UNIX. So most UNIX software installs easily on Mac OS X. Furthermore, Apache and PHP come packaged with Mac OS X. To create a local web server, all you need to do is configure Apache and *install* MySQL.

I am aware of the web server software available for Mac OS X, notably [MAMP][1]. These get you started quickly. But they forego the learning experience and, as most developers report, can become difficult to manage.

## Running Commands

First, open the *Terminal* app and switch to the `root` user so you can run the commands in this post without any permission issues:

    sudo su -

## Enable Apache on Mac OS X

    apachectl start

Verify *It works!* by accessing <http://localhost>

## Enable PHP for Apache
First, make a backup of the default Apache configuration. This is good practice and serves as a comparison against future versions of Mac OS X.

    cd /etc/apache2/
    cp httpd.conf httpd.conf.sierra

Now edit the Apache configuration. Feel free to use *TextEdit* if you are not familiar with *vi*.

    vi httpd.conf

Uncomment the following line (remove `#`):

    LoadModule php5_module libexec/apache2/libphp5.so

Restart Apache:

    apachectl restart

You can verify PHP is enabled by creating a [`phpinfo()`](http://php.net/manual/en/function.phpinfo.php) page in your `DocumentRoot`.

The default `DocumentRoot` for Mac OS X Sierra is `/Library/WebServer/Documents`. You can verify this from your Apache configuration.

    grep DocumentRoot httpd.conf

Now create the `phpinfo()` page in your `DocumentRoot`:

    echo '<?php phpinfo();' > /Library/WebServer/Documents/phpinfo.php

Verify PHP by accessing <http://localhost/phpinfo.php>

## Install MySQL on Mac OS X Sierra

[Download][2] and install the latest MySQL generally available release DMG for Mac OS X.

The **README** suggests creating aliases for `mysql` and `mysqladmin`. However there are other commands that are helpful such as `mysqldump`. Instead, you can [update your path](http://superuser.com/questions/69130/where-does-path-get-set-in-os-x-10-6-snow-leopard) to include `/usr/local/mysql/bin`.

    export PATH=/usr/local/mysql/bin:$PATH

**Note**: You will need to open a new *Terminal* window or run the command above for your path to update.

Finally, you should run `mysql_secure_installation`. While this isn't necessary, it's good practice to secure your database.

## Connect PHP and MySQL
You need to ensure PHP and MySQL can communicate with one another. There are [several options][3] to do so. I do the following:

    cd /var
    mkdir mysql
    cd mysql
    ln -s /tmp/mysql.sock mysql.sock

## Additional Configuration (optional)
The default configuration for Apache 2.4 on Mac OS X seemed pretty lean. For example, common modules like `mod_rewrite` were disabled. You may consider enabling this now to avoid forgetting they are disabled in the future.

I edited my Apache Configuration:

    vi /etc/apache2/httpd.conf

I uncommented the following lines (remove `#`):

    LoadModule deflate_module libexec/apache2/mod_deflate.so
    LoadModule expires_module libexec/apache2/mod_expires.so
    LoadModule rewrite_module libexec/apache2/mod_rewrite.so

If you develop multiple projects and would like each to have a unique url, you can [configure Apache *VirtualHosts* for Mac OS X](/2014/11/configure-apache-virtualhost-mac-os-x/).

If you would like to install [PHPMyAdmin][4], return to my original post on [installing Apache, PHP, and MySQL on Mac OS X](/2012/10/install-apache-php-mysql-mac-os-x/).

 [1]: http://www.mamp.info/en/index.html "MAMP"
 [2]: http://dev.mysql.com/downloads/mysql/
 [3]: http://stackoverflow.com/questions/4219970/warning-mysql-connect-2002-no-such-file-or-directory-trying-to-connect-vi
 [4]: http://www.phpmyadmin.net/ "PHPMyAdmin"
