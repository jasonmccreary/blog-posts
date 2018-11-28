---
title: "Installing Apache, PHP, and MySQL on macOS Mojave"
heading: "Installing Apache, PHP, and MySQL on macOS Mojave"
author: Jason McCreary
excerpt: "This is an update of a previous post to install Apache, PHP, and MySQL on Mac OS for macOS Mojave."
subheading: "This is an update of a previous post to install Apache, PHP, and MySQL on Mac OS for macOS Mojave."
layout: post
comments: true
categories:
  - Main Thread
tags:
  - apache
  - development
  - mysql
  - php
description: "This is an update of a previous post to install Apache, PHP, and MySQL for Mac OS for macOS Mojave ."
subheading: "This is an update of a previous post to install Apache, PHP, and MySQL for Mac OS for macOS Mojave ."
---
_**Note:** This post is for new installations. If you have [installed Apache, PHP, and MySQL for Mac OS Sierra](/2016/09/install-apache-php-mysql-mac-os-x-sierra/), read my post on [Updating Apache, PHP, and MySQL for macOS Mojave](/2018/11/update-apache-php-mysql-mac-os-x-sierra/)._

I am aware of the web server software available for macOS, notably [MAMP][1], as well as package managers like `brew`. These get you started quickly. But they forego the learning experience and, as most developers report, can become difficult to manage.

The thing is macOS runs atop UNIX. So most UNIX software installs easily on macOS. Furthermore, Apache and PHP come preinstalled with macOS. To create a local web server, all you need to do is configure Apache and *install* MySQL.

## Running Commands

First, open the *Terminal* app and switch to the `root` user so you can run the commands in this post without any permission issues:

    sudo su -

## Enable Apache on macOS

    apachectl start

Verify *It works!* by accessing <http://localhost>

## Enable PHP for Apache
First, make a backup of the default Apache configuration. This is good practice and serves as a comparison against future versions of macOS.

    cd /etc/apache2/
    cp httpd.conf httpd.conf.mojave

Now edit the Apache configuration. Feel free to use a different editor if you are not familiar with *vi*.

    vi httpd.conf

Uncomment the following line (remove `#`):

    LoadModule php7_module libexec/apache2/libphp7.so

Restart Apache:

    apachectl restart

You can verify PHP is enabled by creating a [`phpinfo()`](http://php.net/manual/en/function.phpinfo.php) page in your `DocumentRoot`.

The default `DocumentRoot` for macOS Mojave is `/Library/WebServer/Documents`. You can verify this from your Apache configuration.

    grep DocumentRoot httpd.conf

Now create the `phpinfo()` page in your `DocumentRoot`:

    echo '<?php phpinfo();' > /Library/WebServer/Documents/phpinfo.php

Verify PHP by accessing <http://localhost/phpinfo.php>

## Install MySQL on macOS Mojave

[Download][2] and install the latest MySQL generally available release DMG for macOS. While MySQL 8 is the latest version, many of my projects still use [MySQL 5.7](https://dev.mysql.com/downloads/mysql/5.7.html). So I still prefer installing the older version.

When the install completes it will provide you with a temporary password. **Copy this password before closing the installer.** You will use it again in a few steps.

The **README** suggests creating aliases for `mysql` and `mysqladmin`. However there are other commands that are helpful such as `mysqldump`. Instead, you can [update your path](http://superuser.com/questions/69130/where-does-path-get-set-in-os-x-10-6-snow-leopard) to include `/usr/local/mysql/bin`.

    export PATH=/usr/local/mysql/bin:$PATH

**Note**: You will need to open a new *Terminal* window or run the command above for your path to update.

Finally, you should run `mysql_secure_installation`. While this isn't necessary, it's good practice to secure your database. This is also where you can change that nasty temporary password to something more manageable for local development.

## Connect PHP and MySQL
You need to ensure PHP and MySQL can communicate with one another. There are [several options][3] to do so. I like the following as it doesn't require changing lots of configuration:

    mkdir /var/mysql
    ln -s /tmp/mysql.sock /var/mysql/mysql.sock

## Additional Configuration (optional)
The default configuration for Apache 2.4 on macOS seemed pretty lean. For example, common modules like `mod_rewrite` were disabled. You may consider enabling this now to avoid forgetting they are disabled in the future.

I edited my Apache Configuration:

    vi /etc/apache2/httpd.conf

I uncommented the following lines (remove `#`):

    LoadModule deflate_module libexec/apache2/mod_deflate.so
    LoadModule expires_module libexec/apache2/mod_expires.so
    LoadModule rewrite_module libexec/apache2/mod_rewrite.so

If you develop multiple projects and would like each to have a unique url, you can [configure Apache *VirtualHosts* for macOS](/2014/11/configure-apache-virtualhost-mac-os-x/).

If you would like to install [PHPMyAdmin][4], return to my original post on [installing Apache, PHP, and MySQL on macOS](/2012/10/install-apache-php-mysql-mac-os-x/).

 [1]: http://www.mamp.info/en/index.html "MAMP"
 [2]: http://dev.mysql.com/downloads/mysql/
 [3]: http://stackoverflow.com/questions/4219970/warning-mysql-connect-2002-no-such-file-or-directory-trying-to-connect-vi
 [4]: http://www.phpmyadmin.net/ "PHPMyAdmin"
