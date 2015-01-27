---
title: Updating SVN for Mac OS X
author: Jason McCreary
excerpt: >
  This post outlines updating SVN for Mac OS X. I updated after realizing new apps
  used SVN 1.7 while Mac OS X Mountain Lion used SVN 1.6.
layout: post
comments: true
permalink: /2012/10/updating-svn-mac-os-x/
categories:
  - Main Thread
description: This post outlines updating SVN for Mac OS X. I updated after realizing new apps used SVN 1.7 while Mac OS X Mountain Lion used SVN 1.6.
---
***Update:** The Command Line Tools for [Xcode 5](https://developer.apple.com/xcode/) include SVN 1.7.10.*

I downloaded the latest version of [Eclipse][1] and [Subclipse][2] for my new work Macbook Pro. When I ran `svn` commands in *Terminal* I received some odd messages. After some confusion, I realized Subclipse checked out the repository using SVN version 1.7. Unfortunately Mac OS X Mountain Lion runs SVN version 1.6.

I could have downgraded Subclipse. However, I had already checked out several repositories. Furthermore, I liked the smaller footprint of SVN 1.7. In typical lazy developer fashion, I went with updating SVN to version 1.7 for Mac OS X.

To give due credit, the foundations of this post came from a post on [Building SVN 1.7][3]. Although I expanded on it, I encourage you to read the original post. For completeness, I outlined the full process below.

**Note**: To compile and install SVN 1.7 you need [Xcode with the Command Line Tools installed](http://stackoverflow.com/questions/9329243/xcode-4-4-command-line-tools).

## Download the SVN Source

    cd ~/Downloads/
    curl -o subversion-latest.tar.gz http://apache.mirrors.tds.net/subversion/subversion-1.7.8.tar.gz
    tar -xvf subversion-latest.tar.gz

**Note**: You may need to update the `curl` command to download the [latest SVN 1.7 source][1].

## Build and Install SVN
The default SVN install on Mac OS X uses neon. neon allows you to connect to remote SVN repositories via HTTP and HTTPS. Lines 2-7 installs neon. Line 8 builds SVN using the `--with-neon` configuration flag.

    cd ~/Downloads/subversion-1.7.*
    sh get-deps.sh neon
    cd neon/
    ./configure --with-ssl
    make
    sudo make install
    cd ..
    ./configure --prefix=/usr/local --with-neon
    make
    sudo make install
    

## Using the New SVN

Your environment will still use SVN version installed with Mac OS X:

    svn --version

To use the SVN version you just installed, you can [update your `PATH`][4]. Assuming you are using the bash shell, add or edit the following line in your `~/.bash_profile`:

    export PATH=/usr/local/bin:$PATH

You should now see the SVN version you installed:

    svn --version

 [1]: http://subversion.apache.org/download/
 [2]: http://subclipse.tigris.org
 [3]: http://nicoduplessis.com/blog/2012/05/06/building-svn-1-dot-7-on-mac-os-x-lion/
 [4]: http://www.tech-recipes.com/rx/2621/os_x_change_path_environment_variable/
