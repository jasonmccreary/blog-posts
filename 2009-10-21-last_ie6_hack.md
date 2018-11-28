---
title: "My Last IE6 Hack"
heading: "My Last IE6 Hack"
author: Jason McCreary
excerpt: "Today I wrote my last IE6 hack, to overcome a common CSS bug, and I say good riddance to this nearly decade old browser."
subheading: "Today I wrote my last IE6 hack, to overcome a common CSS bug, and I say good riddance to this nearly decade old browser."
layout: post
comments: true
permalink: /2009/10/last_ie6_hack/
tags:
  - development
  - front-end
  - rant
---
Internet Explorer 6 (IE6) is the dreaded web browser that still lingers in existence. It was first released August 27, 2001. That is over 8 years old. Nearly a decade. But as Microsoft's web browser it was widely distributed. 

Nonetheless, in the past two years Microsoft has released two major versions of IE. During which time many high-profile websites have discontinued support for IE6. I have yet to do so, but today I wrote my last IE6 hack, a CSS rule:

    /* my last IE6 hack */
    * html #client_logos li {
       display: inline;
    }
    

## What does this mean?

This means I will not allow IE6 to factor into decisions. In addition, I will no longer develop additional styles or code to support IE6.

## What's the big deal?

The web is a constantly evolving landscape and IE6 is old. It arguably should have died naturally by now. I won't go into the fact that IE6 is poorly developed browser that didn't even support web standards of its day, much less present day. Simply put, supporting IE6 is a ridiculous waste of time. I admittedly have a low tolerance for such things. In addition, it hinders the overall design and development of a website.

## What about Bob? (IE6 Users)

You should upgrade.

## What about client websites?

For client work, I may not have the luxury to completely abandon IE6. I can hope to make educate a client of missed opportunities or the cost of developing for IE6. Yet if a *true* need exists, I will continue to support the browser. In the end, if you write semantic, valid front-end code there are only a handful of IE6 bugs. All of which are well documented and, in my opinion, any web developer has come across several times before.

## Conclusion

I by no means think this is a bold movement. Yet, this decision should not be made on a whim. You should consider development time, designs, usage statistics before making your own.
