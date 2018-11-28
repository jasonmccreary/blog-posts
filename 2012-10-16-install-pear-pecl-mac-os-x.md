---
title: "Install PEAR and PECL on Mac OS X"
heading: "Install PEAR and PECL on Mac OS X"
author: Jason McCreary
excerpt: "This post outlines how to install PEAR and PECL on Mac OS X."
subheading: "This post outlines how to install PEAR and PECL on Mac OS X."
layout: post
comments: true
permalink: /2012/10/install-pear-pecl-mac-os-x/
categories:
  - Main Thread
tags:
  - development
  - mac
  - php
description: "This post outlines how to install PEAR and PECL on Mac OS X."
subheading: "This post outlines how to install PEAR and PECL on Mac OS X."
---
The following instructions install [PEAR][1] and [PECL][2] on Mac OS X under **/usr/local/**. PECL is bundled with PEAR. So this is as simple as installing PEAR on Mac OS X.

PEAR is PHP's *Package* Repository and makes it easy to download and install PHP tools like [PHPUnit][3] and [XDebug][4]. I specifically recommend these two for every PHP developer.

## Download PEAR

    curl -O http://pear.php.net/go-pear.phar
    sudo php -d detect_unicode=0 go-pear.phar
    

## Configure and Install PEAR

You should now be at a prompt to configure PEAR.

1.  Type <kbd>1</kbd> and press <kbd>return</kbd>.
2.  Enter:
    
        /usr/local/pear

3.  Type <kbd>4</kbd> and press <kbd>return</kbd>.
4.  Enter:
    
        /usr/local/bin

5.  Press <kbd>return</kbd>

## Verify PEAR

You should be able to type:

    pear version
    

Eventually, if you use any extensions or applications from PEAR, you may need to update [PHP's include path][5].

 [1]: http://pear.php.net "PEAR"
 [2]: http://pecl.php.net "PECL"
 [3]: https://github.com/sebastianbergmann/phpunit
 [4]: http://xdebug.org
 [5]: http://pear.php.net/manual/en/installation.checking.php
