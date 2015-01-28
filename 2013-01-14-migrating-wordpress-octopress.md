---
layout: post
title: "Migrating from WordPress to Octopress"
author: Jason McCreary
date: 2013-01-14 15:21
comments: true
categories:
  - Main Thread
tags:
  - wordpress
  - octopress
  - ec2
  - jekyll
excerpt: A post on my recent migration from WordPress to Octopress as a return to simple blogging and a site with greasy, fast speed.
description: A post on my recent migration from WordPress to Octopress as a return to simple blogging and a site with greasy, fast speed.
---
This past weekend I migrated my blog from WordPress to Octopress. Before I explain the migration, let me explain why I migrated away from WordPress.

First, I am a fan of WordPress. I've written many [posts on WordPress](http://www.google.com/search?q=site%3Ajason.pureconcepts.net&q=wordpress "Read my WordPress posts"), [spoken at WordCamps](http://www.google.com/search?q=site%3Ajason.pureconcepts.net&q=wordcamp "My talks at WordCamps"), and will continue to develop with WordPress. But [WordPress is slow](http://jason.pureconcepts.net/2012/08/21-ways-wordpress-fast/ "21 Ways to Make WordPress Fast"). And I had a need&hellip; a need for speed. Greasy, fast speed.

Enter [Octopress](http://octopress.org "Octopress"):

> Octopress is a framework designed for [Jekyll](http://github.com/mojombo/jekyll "Jekyll") - the blog aware static site generator powering Github Pages.

**Github** uses it &mdash; that's reassuring. **Static site** &mdash; you don't get much faster than serving a static resource. Look for a follow-up post with the performance showdown between Octopress and WordPress soon.

I also wanted something simple. I don't use all the features of WordPress. I just write posts from time to time. From [the beginning](http://jason.pureconcepts.net/2008/09/hello_world/ "My first blog") I've drafted my posts in Markdown. Jekyll uses [Markdown](http://daringfireball.net/projects/markdown/ "Markdown") in its templates. *Simple*.

You may be wondering the difference between Jekyll and Octopress. Jekyll is tool, Octopress is the packaging. Octopress nicely wraps some of the rough edges of Jekyll making it easier to manage. It also offers [themes](https://github.com/imathis/octopress/wiki/3rd-Party-Octopress-Themes "Octopress Themes") and [plugins](https://github.com/imathis/octopress/wiki/3rd-party-plugins "Octopress Plugins").

Now if, like me, you're sold on Octopress read on. If not, thanks for reading this far. Long live WordPress!

## Migrating from WordPress to Octopress
Here is an outline of the steps for migrating from WordPress to Octopress with more details below. 

1. Installed Octopress
2. Migrated Content from WordPress
3. Previewed Site
4. Deploy Octopress

### Installing OctoPress
For the most part, I followed the [Octopress Setup](http://octopress.org/docs/setup/ "Octopress Setup"). The only exception being Ruby. I'm on Mac OS X Mountain Lion. So my Ruby version was 1.8.7. I updated Ruby with RVM.

    curl -L https://get.rvm.io | bash -s stable --ruby

### Migrating Content from WordPress to Octopress/Jekyll
There were [several migration options](https://github.com/mojombo/jekyll/wiki/blog-migrations "Migrating from WordPress to Jekyll"). I tried a few of them and found [Export to Jekyll](https://github.com/benbalter/wordpress-to-jekyll-exporter "Export to Jekyll") best as it ran content through the approriate WordPress filters. While Export to Jekyll was the best of the bunch, it wasn't perfect.

- **Encoding issues.** After moving the files from Export to Jekyll `rake generate` erred about `gsub`. The issue was [UTF-8](https://github.com/imathis/octopress/issues/148). With some debugging, I found several UTF-8 characters in my posts. Mostly smart quotes and other artifacts like EM dashes from Mac OS X. Unfortunately I didn't find a [quick fix](http://pradeepnayak.in/technology/2012/02/16/jekyll-character-encoding-problems/). I ended up replacing these with HTML entities using some `sed` commands. This was the worst part of the migration. And it wasn't that bad.
- **Missing meta data.** Aside from the title, none of the WordPress SEO settings were exported. I wrote a PHP script to add the `description` and `keywords` to the Front Matter. I will likely fork Export to Jekyll soon. If you're interested in these, please leave a comment.
- **Misconfigured comments.** The export did not set `comment` in the Front Matter. I fixed this with a quick `sed` command as all my posts allow comments. However, this is something Export to Jekyll *could* have done.
- **Exporting comments.** I migrated my [WordPress comments to Disqus](http://help.disqus.com/customer/portal/articles/466255-exporting-comments-from-wordpress-to-disqus "Export WordPress comments to Disqus"). Suprisingly simple. But Disqus took several hours to process the import (130 comments).
- **Static resources**. I needed to download the static resources in my content (images, PDFs, etc). This is not necessary if you use a static domain. But if you use WordPress **uploads/**, you'll need to download them to Octopress **source/**. I wrote a `curl` script to do so.

### Deploying Octopress
Octopress has a few [automated deployment options](http://octopress.org/docs/deploying/ "Deploying Octopress"). I used `rsync`. However, my blog runs on Amazon EC2. So I needed to [configure my EC2 private key to deploy Octopress](https://github.com/mneorr/octopress/commit/7c9c4bad48d921e94a57c63167f88c95f10dd687 "Set Octopress ssh-key"). After doing so, I deployed to Octopress to my Amazon EC2 instance by simply typing `rake deploy`.


## Blogging with Octopress
Octopress is not without its own challenges. I found limited SEO out-of-the-box and I will need to learn the nuiances of Octopress themes. Look for future posts on both.

Nonetheless, with Octopress I can write like I always have. I don't have to manage WordPress upgrades or plugins. I don't have to make WordPress faster. I just write. This frees up my time for other things.
