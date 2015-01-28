---
title: "CakePHP not inheriting custom app_controller and app_model"
heading: "CakePHP not inheriting custom app_controller and app_model"
author: Jason McCreary
excerpt: "My CakePHP project fell down a rabbit hole earlier after creating a custom app_controller and app_model from something that should have been a no-brainer. Hopefully this helps someone else."
subheading: "My CakePHP project fell down a rabbit hole earlier after creating a custom app_controller and app_model from something that should have been a no-brainer. Hopefully this helps someone else."
layout: post
comments: true
permalink: /2011/03/cakephp-custom-app-controller/
categories:
  - Main Thread
tags:
  - cakephp
  - development
  - rabbit-hole
description: My CakePHP project crashed earlier after creating a custom app_controller and app_model from something that should have been a no-brainer.
keywords: cakephp, custom, app_controller, app_model
---
My CakePHP project fell down a rabbit hole earlier over something in hindsight was a no-brainer. I created a custom `app_controller.php` and `app_model.php` in my app directory. I copied the respective files from `cake/libs/controller` and `cake/libs/model`. I added my customizations and refreshed the page. *Nothing.* I checked the filenames and output a few `debug()` calls. *Still nothing.* I added `beforeFilter()` with just an `echo`. *Nothing!* My controllers and models weren&rsquo;t inheriting any of the custom parent methods. Finally, it hit me – **clear the cache**. *It worked.*

Maybe that is a rookie mistake. Nonetheless, hopefully that saves someone the 15 minutes I lost. Clearing the cache was actually the solution to a problem a few months back. Which is actually the only reason I tried it. So when in doubt with CakePHP, clear the cache.
