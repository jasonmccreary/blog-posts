---
title: 'My Talk at WordCamp Chicago: Configuring WordPress for Multiple Environments'
author: Jason McCreary
excerpt: >
  A follow up post about my talk on Configuring WordPress for Multiple
  Environments at WordCamp Chicago. It includes a link to the original blog post,
  slides, code samples, and a note about the proposed Unconference Talk.
layout: post
comments: true
permalink: /2011/07/wordcamp-chicago-talk-configuring-multiple-environments-wordpress/
categories:
  - Main Thread
tags:
  - talks
  - wordpress
description: A follow up post about my talk on Configuring WordPress for Multiple Environments at WordCamp Chicago.
keywords: wordcamp, chicago, configuring wordpress, multiple environments, jason mccreary, speaker, talk
---
***UPDATE:** This talk has been posted on [WordPress.tv][1].*

I just completed my talk on [Configuring WordPress for Multiple Environments][2] at [WordCamp Chicago][3]. Probably close to a hundred in the crowd. Being only my second talk, it was a bit intimidating. But the feedback has been positive. So much so that I&rsquo;ve been asked to release my code samples and slides immediately.

## The Talk

The talk was an evolution from an earlier blog post of the same name – [Configuring WordPress for Multiple Environments][4]. It includes a detailed write-up and code samples (although they might be dated).

## The Slides

The slides for the talk will eventually be on SlideShare. For now you can [download my slides as a PDF][5]. There were several demos. I tried to include a summary slide after each demo slide. Nonetheless, code samples are below and you can always [contact me][6] for more detail.

## The Code

*   WordPress Config File: [wp-config.php][7]
*   Sample WordPress Development Config File: [wp-config.dev.php][8]
*   Sample WordPress Production Config File: [wp-config.prod.php][9]

### Environment Awareness

The following demonstrate using the `VIA_ENVIRONMENT` constant to perform environment specific code.

Include Google Analytics in just Production (in theme&rsquo;s `header.php`):

    <?php
    if (VIA_ENVIRONMENT == 'prod') {
    ?>
    <script type="text/javascript">
    var _gaq = _gaq || [];
    _gaq.push(['_setAccount', 'UA-#######-#']);
    _gaq.push(['_trackPageview']);
    (function () {
        var ga = document.createElement('script');
        ga.type = 'text/javascript';
        ga.async = true;
        ga.src = ('https:' == document.location.protocol ? 'https://ssl' : 'http://www') + '.google-analytics.com/ga.js';
        var s = document.getElementsByTagName('script')[0];
        s.parentNode.insertBefore(ga, s);
    })();
    </script>
    <?php
    }
    ?> 

Use minified resources for any non-Development environment (in theme&rsquo;s `header.php`):

    <?php
    if (VIA_ENVIRONMENT == 'dev') {
    ?>
    <link rel="stylesheet" type="text/css" media="all" href="<?php bloginfo('template_directory'); ?>/res/css/global.css" />
    <link rel="stylesheet" type="text/css" media="all" href="<?php bloginfo('template_directory'); ?>/res/css/off-season.css" />
    <!--[if lte IE 7]><link rel="stylesheet" href="<?php bloginfo('template_directory'); ?>/res/css/ie.css" type="text/css" media="all" /> <![endif]-->
    <?php
    }
    else {
    ?>
    <link rel="stylesheet" type="text/css" media="all" href="<?php bloginfo('template_directory'); ?>/res/css/global-1.0.min.css" />
    <link rel="stylesheet" type="text/css" media="all" href="<?php bloginfo('template_directory'); ?>/res/css/off-season-0.2.min.css" />
    <!--[if lte IE 7]> <link rel="stylesheet" href="<?php bloginfo('template_directory'); ?>/res/css/ie-0.2.min.css" type="text/css" media="all" /> <![endif]-->
    <?php
    }
    ?>

## Setting up a WordPress development environment Unconference

After strong interest I have spoken with the WordCamp Chicago organizers about adding an unconference. We&rsquo;ve been approved to use the open time slot from 9:00-10:00 Sunday morning. I will be helping anyone interested in setting up a local development environment on their Mac. If you&rsquo;d like to do so on a different OS I will try to find additional moderators. Please reach out to me at the after-party or on Twitter, [@gonedark][10], if you plan to attend.

See you at the after-party!

 [1]: http://wordpress.tv/2012/01/24/jason-mccreary-configuring-wordpress-for-multiple-environments/ "Jason McCreary's talk on Configuring WordPress for Multiple Environments"
 [2]: http://2011.chicago.wordcamp.org/session/configuring-wordpress-for-multiple-environments/
 [3]: http://2011.chicago.wordcamp.org/
 [4]: http://viastudio.com/2011/02/08/configuring-wordpress-multiple-environments/
 [5]: /downloads/WordCamp-Chicago-2011.pdf
 [6]: /contact
 [7]: /downloads/wp-config.php.txt
 [8]: /downloads/wp-config.dev.php.txt
 [9]: /downloads/wp-config.prod.php.txt
 [10]: http://twitter.com/gonedark
