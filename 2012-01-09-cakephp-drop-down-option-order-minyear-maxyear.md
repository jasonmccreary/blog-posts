---
title: >
  Unexpected behavior with drop-down option order using minYear, maxYear in
  CakePHP
author: Jason McCreary
excerpt: 'A rant after encountering unexpected sort order when using <code>minYear</code> and <code>maxYear</code> attributes to control options in an drop down. The solution involved an undocumented, and in my opinion unnecessary, attribute.'
layout: post
comments: true
permalink: /2012/01/cakephp-drop-down-option-order-minyear-maxyear/
categories:
  - Main Thread
tags:
  - cakephp
  - rant
description: How to control the sort order of drop-down options when using CakePHP's Form Helper. The solution involved an undocumented attribute.
keywords: cakephp option order minyear maxyear
---
I noticed something interesting when testing a checkout form today build in CakePHP. The options for the drop-down were in descending order. The options were for the credit card expiration date field. The range was set with `minYear` and `maxYear` attributes.

The code in my view (CakePHP version 1.3.7).

    echo $this->Form->input('cc_expires', array('type' => 'date',
      'label' => 'Expiration Date',
      'dateFormat' => 'MY',
      'empty' => true,
      'separator' => '&nbsp;',
      'minYear' => date('Y'),
      'maxYear' => date('Y', strtotime('+7 years')));
    

Although the options range is correct, this seemed unintuitive. In addition, I felt it was slightly poor usability. So I wanted to fix the order.

<figure>
  <img src="/images/cakephp-option-order-default.png" alt="CakePHP default option order" title="The default option order for year in CakePHP" />
  <figcaption>CakePHP default option order</figcaption>
</figure>

I dug around in [The Book][1]. Nothing. I was about to submit a ticket. But before I do, I typically check my version and the core. Upon searching for *minYear* I found the function in question – `year()`. Apparently an **undocumented** attribute exists – `orderYear`. After adding `'orderYear' => 'asc'` to the options array I got the desired output.

Two notes here. First, CakePHP has many undocumented features. It never hurts to dig around the core. Second, the `orderYear` attribute is completely unnecessary in this case. In fact, it is only used for the \*year\* drop-down. It could be easily determined from comparing `minYear` and `maxYear`. In this case, `minYear` is *2012* which is less than `maxYear` is *2019*. Display in ascending order.

<figure>
  <img src="/images/cakephp-option-order-asc.png" alt="Control option order using orderYear for year in CakePHP" title="CakePHP option order using orderYear" />
  <figcaption>CakePHP option order with orderYear</figcaption>
</figure>

Maybe `orderYear` has uses. But today it wasted my time.

    </rant>

 [1]: http://book.cakephp.org/view/876/The-Manual#!/1.3/en/view/1408/options-minYear-options-maxYear
