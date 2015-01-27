---
title: Installing, Configuring, and Deploying I/O Docs
author: Jason McCreary
excerpt: 'This post covers installing, configuring, and deploying (Heroku) I/O Docs - a live interactive documentation system for RESTful web APIs developed with node.js and redis.'
layout: post
comments: true
permalink: /2012/01/iodocs-install-configure-deploy-heroku/
categories:
  - Main Thread
tags:
  - review
  - tools
description: Install, configure, and deploy (Heroku) I/O Docs - a live interactive documentation system for RESTful APIs developed in node.js and redis.
keywords: iodocs, i/o docs, API, documentation, heroku, node.js
---
***Update:** My fork of I/O Docs is now available on [GitHub][1].*

I heard about I/O Docs in a tweet from [@Mashery][2] last August. One of their evangelist developed I/O Docs with node.js and released the project on [Github][3]. I&rsquo;ve wanted to check it out for two reasons. First, I have an undocumented private API – [PocketBracket][4] – that, well, I said it already, was undocumented. Second, I wanted a reason to develop with node.js.

From the I/O Docs synopsis:

> I/O Docs is a live interactive documentation system for RESTful APIs

Some of the highlights of I/O Docs:

*   *live* and *interactive*
*   Promotes RESTful APIs
*   Documentation is done in JSON

## Installing I/O Docs

The README within the project provides pretty good instruction for getting started with I/O Docs. It notes the changes you need to make to each file as well as full detail on how to begin documenting your API using their JSON format.

Unfortunately they gloss over the prerequisites for node.js and redis. If you&rsquo;re running Mac OS X, check out my previous post on [installing node.js, npm, and redis on Mac OS X][5]. Otherwise, the links they provide should get you started.

## Configuring I/O Docs

Out of the box some of the sample APIs did not run. After setting `"debug": true;` in config.json, I noticed these were the API requests only passing an API key. After revisiting GitHub, this was a known issue which lead me to a [fork by ezarko][6].

I applied ezarko&rsquo;s patches to app.js and config.json. This got me most of the way there. I also had to add the following to `unsecuredCall()` (~ line 504):

    options.headers["Content-Length"] = Buffer.byteLength(options.body);
    

Unfortunately the DELETE requests still failed for my PocketBracket API. I realized I was expecting the API key as part of the request body. I/O Docs still appended it to the query params. For a minute I questioned my API architecture. However, using the request body for a DELETE did not violate the [constraints of REST][7] or the [HTTP spec][8]. In fact, to me, it seems more intuitive. In my opinion, only a GET request should explicitly utilize the query params.

I made a few changes to ensure DELETE requests utilized request body properly. I provide my app.js file below. So in summary, I audited the code for DELETE requests and ensured they behaved like PUT/POST (2 places). I also had to modify the `if` statement to ensure the request body was sent with the request (~ line 600).

## Deploying I/O Docs to Heroku

While running I/O Docs locally worked, I needed to share the documentation with my team. Heroku to the rescue. Heroku is a cloud hosting service that plays nicely with git, node.js and redis. In this case, all were free add-ons. The sign up process for Heroku was simple and I was ready to deploy an app in minutes.

I started following a post about deploying I/O Docs to Heroku by [Princess Polymath][9]. Unfortunately as noted in my comment on her post, it didn&rsquo;t get me all the way. Although it ran fine locally, I received an error regarding the redis configuration when running on Heroku.

Heroku required some configuration changes in more spots than Princess Polymath noted. I added the following updates to app.js while to be minimally invasive (I hate hacking core code).

Modify the `config` object before creating the redis connection (~ line 60).

    if (process.env.REDISTOGO<em>URL) {
      // use production (Heroku) redis configuration
      // overwrite <code>config to keep it simple
      var rtg = require(‘url&rsquo;).parse(process.env.REDISTOGO</em>URL);
      config.redis.port = rtg.port;
      config.redis.host = rtg.hostname;
      config.redis.password = rtg.auth.split(&ldquo;:&rdquo;)[1];
    }</code>

Modify the `port` (end of file)

    //  use production (Heroku) port if set 
    var port = process.env.PORT || config.port; 
    app.listen(port, config.address);
    

## Closing

<strike>I plan to start contributing to the I/O Docs project once I become more familiar with git/GitHub (I know)</strike>. My fork of I/O Docs is now available on [GitHub][1]. All other changes should be configuration specific to your environment.

 [1]: https://github.com/jasonmccreary/iodocs "Jason McCreary I/O Docs"
 [2]: https://twitter.com/#!/mashery
 [3]: https://github.com/mashery/iodocs
 [4]: http://www.pocketbracket.com/about
 [5]: http://jason.pureconcepts.net/2011/12/installing-node-js-npm-redis-mac-os-x/
 [6]: https://github.com/ezarko/iodocs
 [7]: http://en.wikipedia.org/wiki/Representational_state_transfer#Constraints
 [8]: http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html
 [9]: http://www.princesspolymath.com/princess_polymath/?p=489
