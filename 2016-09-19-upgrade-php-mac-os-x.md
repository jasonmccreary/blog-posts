---
layout: post
title: "Update PHP on Mac OS X"
date: 2016-09-19 19:37
comments: true
categories:
  - Main Thread
description: 'How to upgrade or install a different version of PHP on Mac OS X.'
subheading: 'How to upgrade or install a different version of PHP on Mac OS X.'
excerpt: 'How to upgrade or install a different version of PHP on Mac OS X.'
---
As noted in my posts on *installing Apache, PHP and MySQL on Mac OS X*, Mac OS X comes pre-installed with Apache and PHP. Unfortunately, as of Mac OS X 10.11 (*El Capitan*) the pre-installed version of PHP is still 5.5.

As PHP 5.5 has reached [end of life](http://php.net/supported-versions.php), the pre-installed version of PHP in Mac OS 10.12 (*Sierra*) is still only PHP 5.6.

So what do you do if you want to upgrade or install a different PHP version on your Mac? Well, you could use [Homebrew](http://brew.sh). But I found a pre-packaged alternative - [PHP OSX](http://php-osx.liip.ch/).

PHP OSX is a package installer for PHP versions 5.3 to 7.1 (current). It's available for Mac OS 10.6+ (*Snow Leopard* to *El Capitan*). While installing PHP OSX is just a few steps, I will walk you through them to add this post to my others.

## Installing PHP
First, choose the version of PHP you want to install. In this example, I will install PHP 7 as that is the latest stable version of PHP.

```sh
curl -s http://php-osx.liip.ch/install.sh | bash -s 7.0
```

If you are not comfortable executing scripts from the Internet, you can do the [install by hand](http://php-osx.liip.ch/#alt_installation).

## Configuring Apache
Provided you are using the pre-installed version of Apache, PHP OSX will automatically configure Apache to use the new PHP module.

Otherwise, you will need to configure Apache to load the newly installed PHP module.

```
LoadModule php5_module /usr/local/php5/libphp5.so
```

## Updating your `PATH`
While Apache will now run the new version of PHP, the command line will not. In order for the command line to use the new version of PHP you will need to update your `PATH`.

```sh
export PATH=/usr/local/php5/bin:$PATH
```

If you don't want to run the command above **every time** you open a new command line, you can update the `PATH` in your `.bash_profile`.

```sh
vi ~/.bash_profile
```

## Configuring PHP
Finally, you will want to update some of the PHP configuration values. PHP OSX installs a PHP INI file for you to change. To edit this file, run:

```sh
sudo vi /usr/local/php5/php.d/99-liip-developer.ini
```

If you kept all of your local PHP configuration within a single INI file (as I did), you can simply append it to the PHP OSX file with:

```sh
sudo cat /Library/Server/Web/Config/php/local.ini >> /usr/local/php5/php.d/99-liip-developer.ini
```

That's it! Now you'll just need to review your PHP code to ensure it's compatible with your newly installed PHP version. And for that, I recommend [PHP Shift](https://php-shift.com).
