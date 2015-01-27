---
title: '&quot;Hello World&quot;'
author: Jason McCreary
excerpt: "If you didn't guess from the title, this is my first article. What does it contain? A look back at the development of this site, of course. I outline the pieces of this site with general development tips."
layout: post
comments: true
permalink: /2008/09/hello_world/
tags:
  - development
  - philosophy
---
Well it took a few months, but I finally launched. For a while I have wanted a personal site for my development articles. I don&rsquo;t plan to publish articles regularly, so I wasn&rsquo;t after a blog. Plus, I am a DIY guy. Even though there are hundreds of blogs available, I only required a few features, some customized. To build or to "buy", that is the question. And in true developer fashion, I built my own.

### The Design

I am a developer, not a designer. I may have a vision, which can be frustrating because it takes me forever and never looks like I right. Yet, for this site, I wanted a [Web 2.0 Design][1] (not that I agree with this definition). Simply a few gradients, badge logo, drop shadows, clean web font, and a bright color. When design falls in my hands the following links provide me enough samples, tutorials, and inspiration to get the job done:

*   [CSS Zen Garden][2]
*   [ColourLovers][3]
*   [99 Web 2.0 Resources][4]

### The Code

Beneath the design lies the code, holding it all together. With due respect, a good design makes a site. But a good design can&rsquo;t stand alone (I must stay true to my development colors, err, code). Anyway, there are two layers of code, by industry standards: front-end and back-end. Front-end, the high level of code, renders the design, behavior, and some simple functionality. Back-end, the lower level, stores data and performs more advanced functionality. Some blur these lines. Others demand segregation. I look at it like food on a plate. When you are hungry, you don&rsquo;t care, it can all run together. Yet, when you make it or pay for it, you want it to look good. 

#### Front-End

A few years ago I swore off table based layouts and migrated towards the Semantic Web. It&rsquo;s amazing how much tag bloat existed. I normally see a reduction of 40%-60%. Even today, I still refine tags when developing a site. At the end of the day, less code is less work. 

When it comes to front-end code I validate HTML 4.01 Strict and CSS 2.1. I am a fan of the Strict Doctype. I like the subset of tags and attributes. Not as restrictive as XHTML Strict, but limiting enough to force basic usage of CSS. I haven&rsquo;t made the shift to XHTML yet, mainly because of browser inconsistencies.

I am currently not using JavaScript heavily, but if I were [Prototype][5] would be on the scene.

My two cents on front-end development:

*   Make sure your [HTML and CSS validates][6], it can save lives.
*   Equalize the browser playing field with a reset stylesheet. I use [Eric Meyer&rsquo;s][7]
*   Develop for [Firefox][8], with [Firebug][9], test in the rest.
*   When you complete the above, check out the [YSlow Rules][10]

#### Back-End

Over the years I have developed the back-end with many languages. Some of those languages have come and gone, some I got paid to use, some aren&rsquo;t even web languages. The one still standing and evolving with me is [PHP][11]. I like the scripting syntax. I am not a fan of tag based back-end languages. They clutter up the front-end code in my opinion. PHP handles the session management, templating, and database interaction for this site. Since I have used PHP for so long I have a collection of custom tools for [Staying DRY][12]. For the database, I use [MySQL][13] for the same reasons as PHP – free and familiar.

### The Challenges

There were not too many pieces of this site that were challenging. In fact, had I gone the "buy" route, these would be moot. Of course, I wanted to do it myself. So, two of the more difficult pieces were the [CAPTCHA][14] and marking up an article.

#### CAPTCHA

I evaluated several PHP versions of a CAPTCHA. I started out with a library download from someone&rsquo;s blog which didn&rsquo;t work (sorry, no plug for you). I then tried [reCAPTCHA][15]. reCAPTCHA worked, but seemed heavy. Sign up for a API key. Download files. Configure. Moreover, it used an `<iframe>` and JavaScript. Finally, I couldn&rsquo;t control the module. As much as I appreciated the built-in functionality, I just needed the CAPTCHA image. Older versions were that simple, but they "grew-up". This combination lead to a thumbs-down for reCAPTCHA.

By then I knew how a CAPTCHA worked. So what did I do – you know it – I built my own. I opened up Fireworks and saved a blank PNG as a base background image. I used native [PHP image functions][16] to overlay a random string onto my base image. In the end, it was only a few lines:

    <?php
    require 'WEBROOT/scripts/init.php';
    
    $output = 'abcdefghijkmnopqrstuvwxyzABCDEFGHJKMNPQRSTUVWXYZ23456789';
    $output = substr(str_shuffle($output), 0, 5);
    
    $_SESSION['captcha'] = $output;
    
    $im = imagecreatefrompng(WEBROOT . '/images/images/captcha.png');
    imagettftext($im, 24.0, 0, 10, 40, imagecolorallocate($im, 0x55, 0x88, 0xAA), WEBROOT . '/images/verdana.ttf', $output);
    
    header('Content-type: image/png');
    imagepng($im); 
    imagedestroy($im);
    ?>
    

A few things to note. First, I store the CAPTCHA string in the session – initialized in init.php – to verify later. Second, I uploaded a true type font file in order to customize the text output. Finally, I output the image directly. This allows me to put my CAPTCHA anywhere on the site with:

`<img src="/includes/captcha.php" alt="Captcha" />`

Admittedly, this is not the greatest. I could have changed text color, size, and rotation. However, for sending comments and feedback on my little site, this should do the job.

I did notice during testing that something may be amiss using the `$_SESSION` in certain browsers (Safari). I believe this steams from the `<img>` source request. I imagine certain browsers may not send session information on these requests for security reasons. Any [feedback][17] on this is appreciated. For now, something to keep in mind.

On a geek note, using the [data: URL scheme][18] was my first choice, but it lacked support in IE and you still can&rsquo;t ignore their market share.

#### Article Markup

With my articles primarily technical, I needed something to format my article text and highlight syntax in code samples. I figured I could just use HTML. Why not? It is for the Web anyway, right? But what if I needed the article in another format, an RSS feed or PDF? Furthermore, if I wrote the article in HTML, why generate my page with PHP from a database. You see how that became a slippery slope. I needed something simpler. I thought about using message board markup tags like `[B]` for **bold**. That seemed to leave me in the same predicament as HTML. I thought about using [LaTeX][19] or another variant. The learning curve seemed steep, although it did offer built-in syntax highlighting. I did some [Google][20] searches and found several possibilities. In the end, they all seemed too heavy. I stepped back and worked on the rest of the site. Then, I stumbled upon [Markdown][21]

> Markdown is a text-to-HTML conversion tool for web writers. Markdown allows you to write using an easy-to-read, easy-to-write plain text format, then convert it to structurally valid XHTML (or HTML).

It looked promising, and I had unknowingly used its basic syntax for years in my README and TODO docs. Immediately, it provided a simple, intuitive syntax without limiting my output. In addition, it&rsquo;s support for inline and block code provided me a foundation for syntax highlight.

### Closing

I know this site could have been built in a day with [Blogger][22] or [WordPress][23]. What made it worse, as a personal project, it took a backseat to [my other work][24]. But as a developer, I wanted to build my own, it&rsquo;s the developer&rsquo;s curse. Hey, I did adopt Markdown. The thing to emphasize is the value of first-hand knowledge. My DIY attitude now will provide a foundation to make stronger decisions to build or "buy" such things in the future.

 [1]: http://www.webdesignfromscratch.com/web-2.0-design-style-guide.cfm
 [2]: http://csszengarden.com
 [3]: http://colourlovers.com
 [4]: http://vandelaydesign.com/blog/design/web-20-design/
 [5]: http://prototypejs.org
 [6]: http://validator.w3.org
 [7]: http://meyerweb.com/eric/thoughts/2007/05/01/reset-reloaded/
 [8]: http://www.mozilla.com/firefox/
 [9]: http://getfirebug.com/
 [10]: http://developer.yahoo.com/yslow/
 [11]: http://www.php.net
 [12]: http://www.phparch.com/c/magazine/issue/45
 [13]: http://www.mysql.com
 [14]: http://en.wikipedia.org/wiki/Captcha
 [15]: http://recaptcha.net/
 [16]: http://www.php.net/manual/en/book.image.php
 [17]: #comments
 [18]: http://developer.mozilla.org/en/The_data_URL_scheme
 [19]: http://www.tug.org/
 [20]: http://google.com
 [21]: http://daringfireball.net/projects/markdown/
 [22]: http://blogger.com
 [23]: http://wordpress.com
 [24]: http://pureconcepts.net
