---
title: "Updating Apache, PHP, and MySQL for macOS Mojave"
heading: "Updating Apache, PHP, and MySQL for macOS Mojave"
author: Jason McCreary
excerpt: "So you upgraded from Mac OS X Sierra for macOS Mojave, huh?"
subheading: "So you upgraded from Mac OS X Sierra for macOS Mojave, huh?"
layout: post
comments: true
categories:
  - Main Thread
tags:
  - apache
  - development
  - mysql
  - php
description: "This post is for those who followed installing Apache, PHP, and MySQL on Mac OS X Sierra and upgraded to macOS Mojave."
---
_**Note:** This post assumes you followed [installing Apache, PHP, and MySQL on Mac OS X Sierra](/2016/09/install-apache-php-mysql-mac-os-x-sierra/) and have since upgraded to macOS Mojave. If you did not follow the original post, you should follow [installing Apache, PHP, and MySQL on macOS Mojave](/2018/11/install-apache-php-mysql-mac-os-x-mojave/)._

When Mac OS X upgrades it overwrites previous configuration files. However, before doing so it will make backups. The backup files often have a suffix of `previous` or `pre-update`. Most of the time, configuring your system after updating Mac OS X is simply a matter of comparing the new and old configurations.

This post will look at the differences in Apache, PHP, and MySQL between Mac OS X Sierra and macOS Mojave.

## Updating Apache
Mac OS X Sierra and macOS Mojave both come with Apache pre-installed. As noted above, your Apache configuration file is overwritten me when you upgrade to macOS Mojave.

There were a few differences in the configuration files. However, since both Sierra and Mojave run Apache 2.4, you could simply backup the configuration file from Mojave and overwrite it with your Sierra version.

    sudo cp /etc/apache/httpd.conf /etc/apache/httpd.conf.mojave
    sudo mv /etc/apache/httpd.conf.pre-update /etc/apache/httpd.conf

However, I encourage you to stay up-to-date. As such, you should take the time to update Mojave's Apache configuration. First, create a backup and compare the two configuration files for differences.

    sudo cp /etc/apache/httpd.conf /etc/apache/httpd.conf.mojave
    diff /etc/apache/httpd.conf.pre-update /etc/apache/httpd.conf

Now edit the Apache configuration. Feel free to use a different editor if you are not familiar with *vi*.

    sudo vi /etc/apache/httpd.conf

Uncomment the following line (remove `#`):

    LoadModule php7_module libexec/apache2/libphp7.so

In addition, uncomment or add any lines you noticed from the `diff` above that may be needed. For example, I uncommented the following lines:

    LoadModule deflate_module libexec/apache2/mod_deflate.so
    LoadModule expires_module libexec/apache2/mod_expires.so
    LoadModule rewrite_module libexec/apache2/mod_rewrite.so

Finally, I cleaned up some of the backups that were created during the macOS Mojave upgrade. This will help avoid confusion in the future.

    sudo rm /etc/apache2/httpd.conf.pre-update
    sudo rm /etc/apache2/extra/*~previous
    sudo rm -rf /etc/apache2/original/

**Note:** These files were not changed between versions. However, if you changed them, you should compare the files before running the commands.

Restart Apache:

    apachectl restart

## Updating PHP
Mac OS X Sierra came with PHP version 5.6 pre-installed. This PHP version has reached its [end of life](http://php.net/supported-versions.php). macOS Mojave comes with PHP 7.1 pre-installed. If you added any extensions to PHP you will need to recompile them.

Also, if you changed the core PHP INI file it will have been overwritten when upgrading to macOS Mojave. You can compare the two files by running the following command:

    diff /etc/php.ini.default /etc/php.ini.default.pre-update

**Note:** Your file may note be named `/etc/php.ini.default.pre-update`. You can see which PHP core files exist by running `ls /etc/php.ini*`.

I would encourage you not to change the PHP INI file directly. Instead, you should overwrite PHP configurations in a custom PHP INI file. This will prevent Mac OS X upgrades from overwriting your PHP configuration in the future. To determine the right path to add your custom PHP INI, run the following command:

    php -i | grep additional

## Updating MySQL
MySQL is not pre-installed with Mac OS X. It is something you downloaded when following the original post. As such, the macOS Mojave upgrade should not have changed your MySQL configuration.

You're good to go.
