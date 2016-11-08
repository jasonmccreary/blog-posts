---
title: "Install siege on Mac OS X"
heading: "Install siege on Mac OS X"
author: Jason McCreary
excerpt: "I've taken an interested in performance lately and heard about siege. The blog articled I found to install siege failed on the first step. So I decided to write my own in hopes it would help any non sysadmin developer that wanted to install siege on Mac OS X Lion. Although this should work with Leopard and Snow Leopard."
subheading: "I've taken an interested in performance lately and heard about siege. The blog articled I found to install siege failed on the first step. So I decided to write my own in hopes it would help any non sysadmin developer that wanted to install siege on Mac OS X Lion. Although this should work with Leopard and Snow Leopard."
layout: post
comments: true
permalink: /2011/09/installing-siege-mac-os-x-lion/
categories:
  - Main Thread
tags:
  - development
  - tools
description: I recently heard about siege, a benchmarking tool, and wanted to install siege on Mac OS X. This post covers the installation process.
keywords: siege, install, mac os x, lion
---
I am not much of a sysadmin. So the process of building from source (`configure` and `make`) remains intimidating. I do what most people would do – went to Google and searched for &ldquo;installing siege on mac os x&rdquo;.

One result looked promising. I attempted to run *Step 1* on the command line. Error. If *Step 1* fails, move on.

To step back, [siege][1] is an http load testing and benchmarking utility. Lately I&rsquo;ve taken a strong interest in benchmarking my web applications. Mainly because I am developing APIs and using WordPress (which is notorious for being slow under server load). Although `ab` (Apache Benchmark) comes bundled with `apache` (which is pre-installed on Mac OS X), I&rsquo;ve been hearing a lot about `siege` at conferences. As any [good developer][2] should, I wanted to tinker with it myself.

## Installing siege

1.  Open the Terminal app
2.  Download the latest version of siege (currently 2.70)

        curl -C - -O http://download.joedog.org/siege/siege-latest.tar.gz

3.  Extract the tarball

        tar -xvf siege-latest.tar.gz

4.  Change directories to the extracted directory (again, currently siege-2.70)

        cd siege-2.70/

5.  Run the following commands (one at a time) to build and install `siege`. If you have an older version of siege read the INSTALL file for more instructions.

        ./configure
        make
        make install

This installed `siege` to `/usr/local/bin/`. This should already be in your `PATH`, so type:

    siege

You may be presented with a message that instructs you to generate a siege configuration file. If so, follow the on screen instructions.

## Benchmarking with siege

The following sends a 10 requests across 10 concurrent connections for benchmarking (no delay between requests).

    siege -c 10 -r 10 -b http://jason.pureconcepts.net/

If you want to learn more about configuring or using siege type `siege -h` or visit the [siege manual][3].

 [1]: http://www.joedog.org/siege-home
 [2]: http://jason.pureconcepts.net/2009/12/good_developer_routines/ "Routines of a good developer"
 [3]: http://www.joedog.org/siege-manual
