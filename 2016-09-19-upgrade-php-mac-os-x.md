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
As noted in my posts on *installing Apache, PHP and MySQL on Mac OS X*, Mac OS X comes pre-installed with Apache and PHP. Unfortunately, often the PHP version pre-installed with Mac OS X is outdated:

- Mac OS 10.11 (*El Capitan*) pre-installed with PHP 5.5
- Mac OS 10.12 (*Sierra*) pre-installed with PHP 5.6
- Mac OS 10.14 (*Mojave*) pre-installed with PHP 7.1

Many of these PHP versions are already [end of life](http://php.net/supported-versions.php). In fact, macOS Mojave was the first time the pre-installed version was _recent_ - although still not the latest PHP version.

So what do you do if you want to upgrade or install a different PHP version on your Mac? Well, you could use [Homebrew](http://brew.sh). But I found a pre-packaged alternative - [PHP OSX](http://php-osx.liip.ch/).

*PHP OSX* is a package installer for PHP versions 5.3 to 7.3 (current). It's available for Mac OS 10.6+ (*Snow Leopard* to *Mojave*). While installing *PHP OSX* is just a few steps, I'll walk you through each of them.

## Installing PHP
First, choose the version of PHP you want to install. In this example, I'll install PHP 7.2 as that is the latest stable version of PHP. However, if you want to install PHP 7.1 that is available as well.

```sh
curl -s http://php-osx.liip.ch/install.sh | bash -s 7.2
```

If you're not comfortable executing scripts from the Internet, you can do the [install by hand](http://php-osx.liip.ch/#alt_installation).

## Configuring Apache
Provided you are using the pre-installed version of Apache, *PHP OSX* will add the `/etc/apache2/other/+php-osx.conf` configuration file which will automatically be loaded by Apache.

If you had previously enabled PHP (as I did), you'll need to comment out the following line in `/etc/apache2/httpd.conf`:

    LoadModule php7_module /usr/local/php5/libphp7.so

If you are running an older version of Mac OS X, the line may be:

    LoadModule php5_module /usr/local/php5/libphp5.so

## Updating your `PATH`
Although Apache will now run the new version of PHP, the command line will not. In order for the command line to use the new version of PHP you will need to update your `PATH`.

```sh
export PATH=/usr/local/php5/bin:$PATH
```

If you don't want to run the command above **every time** you open a new terminal, you can update the `PATH` in your `.bash_profile`.

```sh
vi ~/.bash_profile
```

## Configuring PHP
Finally, you will want to update some of the PHP configuration values. *PHP OSX* installs a PHP INI file for you to change. To edit this file, run:

```sh
sudo vi /usr/local/php5/php.d/99-liip-developer.ini
```

If you kept all of your local PHP configuration within a single INI file (as I did), you can simply append it to the PHP OSX file with:

```sh
sudo cat /Library/Server/Web/Config/php/local.ini >> /usr/local/php5/php.d/99-liip-developer.ini
```

That's it!

Now you'll just need to review your PHP code to ensure it's compatible with your newly installed PHP version. And for that, I recommend [PHP Shift](https://php-shift.com).
