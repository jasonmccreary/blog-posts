---
title: Configuring Apache Virtual Hosts on Mac OS X
author: Jason McCreary
excerpt: Tutorial for configuring Apache Virtual Hosts on Mac OS X.
layout: post
comments: true
categories:
  - Main Thread
description: Tutorial for configuring Apache Virtual Hosts on Mac OS X.
---
*mountaindogmedia* left the following comment on my post for [installing Apache, PHP, and MySQL on Mac OS X](/2012/10/install-apache-php-mysql-mac-os-x/):

> Jason, have you tried a modified `Include` statement for virtual hosts to map a directory? So instead of `/etc/apache2/extra/httpd-vhosts.conf` as indicated, one would use `/etc/apache2/extra/vhosts/*.conf` and then just create a `default.conf` for the first virtual host, and then add/edit/delete vhost files as needed. I think it would be easier to manage host files and changes.

Indeed, *mountaindogmedia*, this is an easier way. In fact, this is the default configuration for many servers.

By default, the Apache Virtual Host configuration on Mac OS X is located in a single file: `/etc/apache2/extra/httpd-vhosts.conf`. You need to edit the Apache configuration to include this file and enable virtual hosts.

Over the years, I have created many virtual hosts. Each time editing `httpd-vhosts.conf`. To *mountaindogmedia's* point, this becomes difficult to manage. Furthermore, Apache configurations often get reset when upgrading Mac OS X. In the same amount of steps (two), you can adopt a more manageable configuration.

## What are Virtual Hosts?
From the [Apache Virtual Host documentation](http://httpd.apache.org/docs/current/vhosts/index.html):

> The term Virtual Host refers to the practice of running more than one web site on a single machine.

By default, the Apache configuration on Mac OS X serves files from `/Library/WebServer/Documents` accessed by the name `locahost`. This is essentially a single site configuration. You could mimic multiple sites by creating subdirectories and access a site at `localhost/somesite`.

This is not ideal for several reasons. Primarily, we would rather access the site using a name like `somesite.local`. To do that, you need to configure virtual hosts.

## A Cleaner Configuration
Before I being, I assume you already [installed and configured Apache on Mac OS X](/2014/11/install-apache-php-mysql-mac-os-x-yosemite/).

First, open the *Terminal* app and switch to the `root` user to avoid permission issues while running these commands.

    sudo su -

Edit the Apache configuration file:

    vi /etc/apache2/httpd.conf

Find the following line:

    #Include /private/etc/apache2/extra/httpd-vhosts.conf

Below it, add the following line:

    Include /private/etc/apache2/vhosts/*.conf

This configures Apache to include all files ending in `.conf` in the `/private/etc/apache2/vhosts/` directory. Now we need to create this directory.

    mkdir /etc/apache2/vhosts
    cd /etc/apache2/vhosts

Create the default virtual host configuration file.

    vi _default.conf

Add the following configuration:

    <VirtualHost *:80>
        DocumentRoot "/Library/WebServer/Documents"
    </VirtualHost>

I create this file to serve as the default virtual host. When Apache can not find a matching virtual host, it will use the first configuration. By prefixing this file with an underscore, Apache will include it first. Techincally this file is not needed as it simply repeats the configuraton already in `httpd.conf`. However, it provides a place to add custom configuration for the default virtual host (i.e. `localhost`).

Now you can create your first virtual host. The example below contains the virtual host configuration for my site. Of course, you will want to substitute *jason.pureconcepts.net* with your domain name.

Create the virtual host configuration file:

    vi jason.pureconcepts.net.conf

Add the following configuration:

    <VirtualHost *:80>
            DocumentRoot "/Users/Jason/Documents/workspace/jason.pureconcepts.net/htdocs"
            ServerName jason.pureconcepts.local
            ErrorLog "/private/var/log/apache2/jason.pureconcepts.local-error_log"
            CustomLog "/private/var/log/apache2/jason.pureconcepts.local-access_log" common
    
            <Directory "/Users/Jason/Documents/workspace/jason.pureconcepts.net/htdocs">
                AllowOverride All
                Require all granted
            </Directory>
    </VirtualHost>

This `VirtualHost` configuration allows me to access my site from *http://jason.pureconcepts.local* for local development.

**Note**: I use the extension *local*. This avoids conflicts with any *real* extensions and serves as a reminder I am developing in my *local* environment.

**Note**: The `Require all granted` configuration became available in Apache 2.4 which comes with OS X Yosemite. If you are running a version of OS X before Yosemite, use the equivalent 2.2 configuration in the [upgrading Apache examples](http://httpd.apache.org/docs/2.4/upgrading.html#run-time).

The final step is to restart Apache:

    apachectl restart

If you run into any problems, run:

    apachectl configtest

This will test your Apache configuration and display any error messages.

## Mapping the .local extension
In order to access sites locally you need to edit your *hosts* file.

    vi /etc/hosts

Add a line to the bottom of this file for your virtual host. It should match the value you used for the `ServerName` configuration. For example, my site:

    127.0.0.1       jason.pureconcepts.local

I like to run the following to clear the local DNS cache:

    dscacheutil -flushcache

Now you can access your site using the .local extension. For example, *http://jason.pureconcepts.local*.

## A note about permissions
You may receive *403 Forbidden* when you visit your local site. This is likely a permissions issue. Simply put, the Apache user (`_www`) needs to have access to read, and sometimes write, to your web directory.

If you are not familiar with permissions, [read more](http://www.library.yale.edu/wsg/docs/permissions/). For now though, the easiest thing to do is ensure your web directory has permissions of `755`. You can change permissions with the command:

    chmod 755 some/web/directory/
    
In my case, all my files were under my local `~/Documents` directory. Which by default is only readable by me. So I had to change permissions from my web directory all the way up to `~/Documents` to resolve the *403 Forbidden* issue.

**Note**: There are many ways to solve permission issues. I have provided this as the *easiest* solution, not the *best*.

## In Closing
Any time you want to add a site to Apache on your Mac, simply create a virtual host configuration file for that site and map it in your *hosts* file.