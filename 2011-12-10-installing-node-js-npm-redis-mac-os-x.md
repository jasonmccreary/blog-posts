---
title: "Installing node.js, npm, and redis on Mac OS X"
heading: "Installing node.js, npm, and redis on Mac OS X"
author: Jason McCreary
excerpt: "After scouring the web for days, this post aims to provide concise, central instructions for installing node.js, npm, and redis on Mac OS X."
subheading: "After scouring the web for days, this post aims to provide concise, central instructions for installing node.js, npm, and redis on Mac OS X."
layout: post
comments: true
permalink: /2011/12/installing-node-js-npm-redis-mac-os-x/
categories:
  - Main Thread
description: This post provides step-by-step instructions for installing node.js, npm, and redis on Mac OS X natively.
---
***Update:** [node.js][1] now has a package installer for Mac OS X which includes node and npm. Unless you need to install node or npm by hand, I suggest downloading the package installer.*

I&rsquo;ve been wanting to mess with [I/O Docs][2] for some time now. I/O Docs requires [node.js][3], [npm][4], and [redis][5]. I hear the buzz around these technologies, but I have yet to use them. Although I found [several posts][6] and a [package][7] for installing node.js and npm on Mac OS X, each had issues. Mac OS X runs atop BSD Unix. So, while potentially intimidating, you can install all these yourself by running commands within Terminal.

## Installing node.js

After much Googling I discovered an overwhelming [set of node.js installation instructions][8]. In a nutshell (no pun), this installs node.js under a newly created `local` folder in the current user folder and adds that folder to your `PATH` so you can run node.js simply by typing <kbd>node</kbd>.

    echo 'export PATH=$HOME/local/bin:$PATH' >> ~/.bash_profile
    source ~/.bash_profile
    mkdir ~/local
    mkdir ~/node-js
    cd ~/node-js
    curl http://nodejs.org/dist/node-v0.4.7.tar.gz | tar xz --strip-components=1
    ./configure --prefix=~/local
    make install
    

A few notes.

First, this installs node.js version 0.4.7. From what I read, this is currently the most compatible version. If you require a different version, I&rsquo;ll assume you know more about installing node.js than me.

Second, bash on Max OS X uses `.bash_profile` not `.bashrc`. I&rsquo;ve modified the [original script][9] to reflect these changes. 

## Installing npm

Once you have installed node.js, you can install npm with just one command.

    curl http://npmjs.org/install.sh | sh
    

I should pass along the warning that this runs commands streamed from the internet. If you&rsquo;re paranoid about that kind of stuff, you should download and verify `install.sh` first.

## Installing redis

Redis was a straightforward install. For the most part I followed the [redis quickstart guide][10]. I modified the script below slightly to use `curl` as Mac OS X does not include `wget`.

    curl -O http://download.redis.io/redis-stable.tar.gz
    tar -xvzf redis-stable.tar.gz
    rm redis-stable.tar.gz
    cd redis-stable
    make
    sudo make install
    

**Note**: `rm redis-stable.tar.gz` is simple cleanup. `sudo make install` is optional as it adds the Redis commands to */usr/local/bin/*.

## In closing

In time, you may need to update the versions for node.js and redis. Both offer a *latest* download. Feel free to substitute these into your script. The commands above should still work. Nonetheless, I tried to provide links to the original documentation when available.

This post came from [getting started with I/O Docs][11]. As I/O Docs required node.js and redis.

 [1]: http://nodejs.org/download/ "Download node.js"
 [2]: https://github.com/mashery/iodocs/
 [3]: http://nodejs.org/
 [4]: http://npmjs.org/
 [5]: http://redis.io/
 [6]: http://www.google.com/search?q=node+on+mac
 [7]: https://sites.google.com/site/nodejsmacosx/
 [8]: https://gist.github.com/579814
 [9]: https://gist.github.com/579814#file_node_and_npm_in_30_seconds.sh
 [10]: http://redis.io/topics/quickstart
 [11]: http://jason.pureconcepts.net/2012/01/iodocs-install-configure-deploy-heroku/ "Installing, Configuring, and Deploying I/O Docs"
