---
title: "Installing Apache, PHP, and MySQL on Mac OS X"
heading: "Installing Apache, PHP, and MySQL on Mac OS X"
author: Jason McCreary
excerpt: "This post outlines installing Apache, PHP, and MySQL on Mac OS X. In addition, I cover configuring Virtual Hosts and installing PHPMyAdmin."
subheading: "This post outlines installing Apache, PHP, and MySQL on Mac OS X. In addition, I cover configuring Virtual Hosts and installing PHPMyAdmin."
layout: post
comments: true
permalink: /2012/10/install-apache-php-mysql-mac-os-x/
categories:
  - Main Thread
tags:
  - apache
  - development
  - mysql
  - php
description: This post outlines installing Apache, PHP, and MySQL on Mac OS X. In addition, I cover configuring Virtual Hosts and installing PHPMyAdmin.
---
***OS X Yosemite Update:** Enough changed in Yosemite to make most of this post obsolete. I wrote a new post for [installing Apache, PHP, and MySQL on Mac OS X Yosemite](/2014/11/install-apache-php-mysql-mac-os-x-yosemite/).*

***OS X Mavericks Update:** I added steps for existing installs which **upgraded** to Mac OS X Mavericks. For new installs of Apache, PHP, and MySQL on Mac OS X Mavericks, continue reading.*

I have installed Apache, PHP, and MySQL on Mac OS X since Leopard. Each time doing so by hand. Each version of Mac OS X having some minor difference. This post serves as much for my own record as to outline how to install Apache, MySQL, and PHP for a local development environment on Mac OS X <s>Mountain Lion</s> Mavericks.

I am aware of the several packages available, notably [MAMP][1]. These packages help get you started quickly. But they forego the learning experience and, as most developers report, eventually break. Personally, the choice to do it myself has proven invaluable.

It is important to remember Mac OS X runs atop UNIX. So all of these technologies install easily on Mac OS X. Furthermore, Apache and PHP are included by default. In the end, you only *install* MySQL then simply turn everything on.

First, open *Terminal* and switch to `root` to avoid permission issues while running these commands.

    sudo su -

## Enable Apache on Mac OS X

    apachectl start

**Note**: Prior to Mountain Lion this was an option for *Web Sharing* in *System Prefrences → Sharing*.

Verify *It works!* by accessing <http://localhost>

## Enable PHP for Apache
***OS X Mavericks Update**: You will need to rerun the steps in this section after upgrading an existing install to Mac OS X Mavericks.*

First, make a backup of the default Apache configuration. This is good practice and serves as a comparison against future versions of Mac OS X.

    cd /etc/apache2/
    cp httpd.conf httpd.conf.bak

Now edit the Apache configuration. Feel free to use *TextEdit* if you are not familiar with *vi*.

    vi httpd.conf

Uncomment the following line (remove `#`):

    LoadModule php5_module libexec/apache2/libphp5.so

Restart Apache:

    apachectl restart


## Install MySQL

1.  [Download][2] the MySQL DMG for Mac OS X
2.  Install *MySQL*
3.  Install *Preference Pane*
4.  Open *System Preferences → MySQL*
5.  Ensure the MySQL Server is running
6.  Optionally, you can enable MySQL to start automatically. I do.

The **README** also suggests creating aliases for `mysql` and `mysqladmin`. However there are other commands that are helpful such as `mysqldump`. Instead, I [updated my path](http://superuser.com/questions/69130/where-does-path-get-set-in-os-x-10-6-snow-leopard) to include `/usr/local/mysql/bin`.

    export PATH=/usr/local/mysql/bin:$PATH

**Note**: You will need to open a new *Terminal* window or run the command above for your path to update.

I also run `mysql_secure_installation`. While this isn&rsquo;t necessary, it&rsquo;s good practice.

### Connect PHP and MySQL
You need to ensure PHP and MySQL can communicate with one another. There are [several options][3] to do so. I do the following:

    cd /var
    mkdir mysql
    cd mysql
    ln -s /tmp/mysql.sock mysql.sock

## Creating *VirtualHosts*

You could stop here. PHP, MySQL, and Apache are all running. However, all of your sites would have URLs like <http://localhost/somesite/> pointing to **/Library/WebServer/Documents/somesite**. Not ideal for a local development environment.

***OS X Mavericks Update**: You will need to rerun the steps below to uncomment the *vhost* `Include` after upgrading an existing install to Mac OS X Mavericks.*

To run sites individually you need to enable *VirtualHosts*. To do so, we'll edit the Apache Configuration again.

    vi /etc/apache2/httpd.conf

Uncomment the following line:

    Include /private/etc/apache2/extra/httpd-vhosts.conf

Now Apache will load **httpd-vhosts.conf**. Let&rsquo;s edit this file.

    vi /etc/apache2/extra/httpd-vhosts.conf

Here is an example of *VirtualHosts* I&rsquo;ve created.

    <VirtualHost *:80>
        DocumentRoot "/Library/WebServer/Documents"
    </VirtualHost>

    <VirtualHost *:80>
            DocumentRoot "/Users/Jason/Documents/workspace/dev"
            ServerName jason.local
            ErrorLog "/private/var/log/apache2/jason.local-error_log"
            CustomLog "/private/var/log/apache2/jason.local-access_log" common

            <Directory "/Users/Jason/Documents/workspace/dev">
                    AllowOverride All
                    Order allow,deny
                    Allow from all
            </Directory>
    </VirtualHost>


The first `VirtualHost` points to `/Library/WebServer/Documents`. The first `VirtualHost` is important as it behaves like the default Apache configuration and used when no others match.

The second `VirtualHost` points to my *dev* workspace and I can access it directly from *http://jason.local*. For ease of development, I also configured some custom logs.

**Note**: I use the extension *local*. This avoids conflicts with any *real* extensions and serves as a reminder I&rsquo;m in my *local* environment.

Restart Apache:

    apachectl restart

In order to access <http://jason.local>, you need to edit your **hosts** file.

    vi /etc/hosts

Add the following line to the bottom:

    127.0.0.1       jason.local

I run the following to clear the local DNS cache:

    dscacheutil -flushcache

Now you can access <http://jason.local>.

**Note:** You will need to create a new `VirtualHost` and edit your **hosts** file each time you make a new local site.

### A note about permissions
You may receive *403 Forbidden* when you visit your local site. This is likely a permissions issue. Simply put, the Apache user (`_www`) needs to have access to read, and sometimes write, your web directory.

If you are not familiar with permissions, [read more](https://www.ics.uci.edu/computing/linux/file-security.php). For now though, the easiest thing to do is ensure your web directory has permissions of `755`. You can change permissions with the command:

    chmod 755 some_directory/

In my case, all my files were under my local `~/Documents` directory. Which by default is only readable by me. So I had to change permissions for my web directory all the way up to `~/Documents` to resolve the *403 Forbidden* issue.

**Note**: There are many ways to solve permission issues. I have provided this as the *easiest* solution, not the *best*.

## Install PHPMyAdmin
Unless you want to administer MySQL from the command line, I recommend installing [PHPMyAdmin][4]. I won&rsquo;t go into the details. Read the installation guide for more information. I install utility applications in the default directory. That way I can access them under, in this case, <http://localhost/phpmyadmin>.

    cd /Library/WebServer/Documents/
    tar -xvf ~/Downloads/phpMyAdmin-3.5.2.2-english.tar.gz
    mv phpMyAdmin-3.5.2.2-english/ phpmyadmin
    cd phpmyadmin
    mv config.sample.inc.php config.inc.php

## Closing

A local development environment is a mandatory part of the [Software Development Process][5]. Given the ease at which you can install Apache, PHP, and MySQL on Mac OS X there really is no excuse.

 [1]: http://www.mamp.info/en/index.html "MAMP"
 [2]: http://dev.mysql.com/downloads/mysql/
 [3]: http://stackoverflow.com/questions/4219970/warning-mysql-connect-2002-no-such-file-or-directory-trying-to-connect-vi
 [4]: http://www.phpmyadmin.net/ "PHPMyAdmin"
 [5]: http://en.wikipedia.org/wiki/Software_development_process
