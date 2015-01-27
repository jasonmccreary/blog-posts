---
title: Installing and Configuring AWStats on Amazon EC2
author: Jason McCreary
excerpt: >
  Step by step instructions for installing and configuring AWStats on Amazon EC2,
  as well as a script that automatically configures and updates sites for AWStats.
layout: post
comments: true
permalink: /2012/03/install-configure-awstats-amazon-ec2/
categories:
  - Main Thread
tags:
  - amazon ec2
  - script
description: Instructions for installing and configuring AWStats on Amazon EC2 and a script that automatically configures and updates sites for AWStats.
---
As admitted before, I am no sysadmin. However, I&rsquo;ve taken an interest in [Amazon EC2][1]. As such, I&rsquo;m learning as I go. This time, it&rsquo;s how to install [AWStats][2]. What&rsquo;s a server without stats, right?

Right now, for learning purposes, I have an EC2 micro instance running Amazon Linux 64-bit. That likely doesn&rsquo;t matter for the install. There are a few conventions I follow:

*   Web data is in */var/www/*
*   Website logs are in */var/logs/httpd/sites/*

With that said, the following steps install and configure AWStats (version 7.0).

1.  Download, extract, and install AWStats
    
        wget http://prdownloads.sourceforge.net/awstats/awstats-7.0.tar.gz
        tar -zxf awstats-7.0.tar.gz 
        sudo mv awstats-7.0/ /var/www/awstats

2.  Enable CGI including the .pl extension under Apache. There are [alternatives][3] if you don&rsquo;t want to enable CGI globally.
    
        sudo vi /etc/httpd/conf/httpd.conf
    
    Change:
    
        #AddHandler cgi-script .cgi
    
    To:
    
        AddHandler cgi-script .cgi .pl

3.  Run the [AWStats Tool][4] and follow the instructions. I followed the defaults and named my server *ec2test*. From what [I read][5], the name doesn&rsquo;t really matter.
    
        sudo perl /var/www/awstats/tools/awstats_configure.pl

4.  Create the [dataDir][6] for AWStats
    
        sudo mkdir /var/lib/awstats/

5.  Enable combined logs. Ensure the `CustomLog` for your sites use a *combined* access log
    
        CustomLog "/var/log/httpd/sites/domain.tld-access_log" combined

6.  Although the AWStats tool does, I restarted Apache again as I made changes in the last step.
    
        sudo service httpd graceful

View AWStats at http://*yourec2domain.com*/awstats/awstats.pl?config=*ec2test*

## Troubleshooting

When I first visited my AWStats, I got a 404. After some digging around, I found the */etc/httpd/conf.d/awstats.conf* was missing the following critical line:

    ScriptAlias /awstats/ "/var/www/awstats/wwwroot/cgi-bin/"
    

AWStats didn&rsquo;t 404. But there was no data. AWStats has an updater you need to run for each of your sites.

    sudo /var/www/awstats/wwwroot/cgi-bin/awstats.pl -update -config=ec2test
    

## Automating AWStats Updates

If you only have one site putting the above in cron is no big deal. But if you have multiple sites with multiple configurations, that&rsquo;s another story. There&rsquo;s a maintenance overhead for making the conf file and then adding the command to cron.

I found a shell script that does all this for you. Essentially, it examines your site logs and ensure that an AWStats configuration exists. Under the assumption if a site has an access log you want AWStats for it. It then runs the AWStats updater for that configuration.

You need to create a conf file to be used as the template when creating new site configuration. I simply copied my main configuration file:

    sudo cp awstats.ec2test.conf template.conf
    

I changed a few lines in the template:

*   All instances of *ec2test* to *domain.tld* (*domain.tld* is a placeholder for the script)
*   Changed the name and location of the access log file (use the placeholder)

Here&rsquo;s the script. You can add it solely to cron – one script to rule them all.

    #!/bin/sh
    
    # find new sites
    sites=$(ls /var/log/httpd/sites/*-access_log)
    for site in $sites
    do
    	domain=$(echo $site | sed -e "s/^\/var\/log\/httpd\/sites\///" -e "s/\-access_log$//")
    	if [ ! -e /etc/awstats/awstats.$domain.conf ]
    	then
    		domainregex=$(echo $domain | sed -e "s/\./\./g")
    		cat /etc/awstats/template.conf | sed -e "s/domain\.tld/$domainregex/g" > /etc/awstats/awstats.$domain.conf
    	fi
    done
    
    awstats="/var/www/awstats/wwwroot/cgi-bin/awstats.pl"
    cd /etc/awstats
    
    # update all sites with configuration files
    for file in $(ls awstats.*.conf)
    do
    	domain=$(echo $file | sed -e"s/^awstats\.//" -e "s/\.conf$//")
    	$awstats -config=$domain -update 
    done
    

Run it:

    sudo sh /etc/awstats/daily.sh

# Closing

There were several good references for installing and configuring AWStats. However, nothing seemed comprehensive or specific for Amazon EC2. So if nothing else, I figured I&rsquo;d post and fill the keyword gap to hopefully help those like me starting our with server admin and Amazon EC2.

 [1]: http://aws.amazon.com/ec2/ "Amazon EC2"
 [2]: http://awstats.sourceforge.net/
 [3]: http://www.electrictoolbox.com/installing-awstats/
 [4]: http://awstats.sourceforge.net/docs/awstats_setup.html
 [5]: http://awstats.sourceforge.net/docs/awstats_config.html#SiteDomain
 [6]: http://awstats.sourceforge.net/docs/awstats_config.html#DirData
