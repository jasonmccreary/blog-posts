---
title: "Updating Apache, PHP, and MySQL to Mac OS X El Capitan"
heading: "Updating Apache, PHP, and MySQL to Mac OS X El Capitan"
author: Jason McCreary
excerpt: "So you upgraded from Mac OS X Yosemite to Mac OS X El Capitan, huh?"
subheading: "So you upgraded from Mac OS X Yosemite to Mac OS X El Capitan, huh?"
layout: post
comments: true
categories:
  - Main Thread
tags:
  - apache
  - development
  - mysql
  - php
description: "This post is for those who followed installing Apache, PHP, and MySQL on Mac OS X Yosemite and upgraded to Mac OS X El Capitan."
---
***Note:** This post assumes you followed [installing Apache, PHP, and MySQL on Mac OS X Yosemite](/2014/11/install-apache-php-mysql-mac-os-x-yosemite/) and have since upgraded to Mac OS X El Capitan. If you did not follow the original post, you should follow [installing Apache, PHP, and MySQL on Mac OS X El Capitan](/2015/10/install-apache-php-mysql-mac-os-x-el-capitan/).*

***PHP Update:** Mac OS X El Capitan comes pre-installed with PHP version 5.5 which has reached its [end of life](http://php.net/supported-versions.php). After you complete this post, you should [upgrade PHP on Mac OS X](/2016/09/upgrade-php-mac-os-x/).*

When Mac OS X upgrades it overwrites previous configuration files. However, before doing so it will make backups. The backup files often have a suffix of `previous` or `pre-update`. Most of the time, configuring your system after updating Mac OS X is simply a matter of comparing the new and old configurations.

This post will look at the differences in Apache, PHP, and MySQL between Mac OS X Yosemite and Mac OS X El Capitan.

## Updating Apache
Mac OS X Yosemite and Mac OS X El Capitan both come with Apache 2.4 preinstalled. As noted above, your Apache configuration file is overwritten me when you upgrade to Mac OS X El Capitan.

Comparing the configuration files show no differences other than the changes made in the original post. As such, you can simply overwrite El Capitan's configuration file with the original by running the following command:

    sudo mv /etc/apache/httpd.conf.pre-update /etc/apache/httpd.conf

## Updating PHP
Both Mac OS X Yosemite and Mac OS X El Capitan run PHP 5.5. However, there is a difference between the minor versions. So if you added any extensions to PHP you will need to recompile them.

Also, if you changed the core PHP INI file it will have been overwritten when upgrading to Mac OS X El Capitan. You can compare the two files by running the following command:

    diff /etc/php.ini.default /etc/php.ini.default.pre-update

**Note:** You may need to change `/etc/php.ini.default.pre-update`. You can see which PHP core files exist by running `ls /etc/php.ini*`.

I would encourage you not to change the PHP INI file directly. Instead, you should overwrite PHP configurations in a custom PHP INI file. This will prevent Mac OS X upgrades from overwriting your PHP configuration in the future. To determine the right path to add your custom PHP INI, run the following command:

    php -i | grep additional

## Updating MySQL
MySQL is not preinstalled with Mac OS X. It is something you downloaded when following the original post. As such, the upgrade should not have changed your MySQL configuration.

MySQL has had a few minor version updates since my original post. If you wish to upgrade MySQL you may do so by following the instructions in [this post](http://coolestguidesontheplanet.com/upgrade-mysql-database-5-5-5-6-osx-10-8-mountan-lion/)



