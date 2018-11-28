---
title: "Guidance Text using CakePHP&#8217;s Form Helper"
heading: "Guidance Text using CakePHP&#8217;s Form Helper"
author: Jason McCreary
excerpt: "A quick post on outputting the proper markup for guidance text using CakePHP's Form Helper."
subheading: "A quick post on outputting the proper markup for guidance text using CakePHP's Form Helper."
layout: post
comments: true
permalink: /2011/04/cakphp_form_helper_guidance_instructional_text/
categories:
  - Main Thread
tags:
  - back-end
  - cakephp
  - front-end
description: A quick post on outputting the proper markup for guidance text using CakePHP's Form Helper.
keywords: cakephp, form helper, guidance text, instruction, input
---
Often form fields have guidance text. Some simple example of how the data should be entered, like *Enter your username*. In the application world (e.g. iPhone) this appears within the field. HTML5 offers this same effect as a new attribute. However, at the moment you would need to implement an [over label solution][1] using CSS and or JavaScript. Either way, the instructional text or markup needs to exist for that form element. Furthermore, if the text were more instructional, you may not want to use `<label>` as the markup.

In CakePHP this presented a challenge. If you are using the [Form Helper][2], the markup is generated for you. I wanted a solution that would inject my guidance text or instructional markup into CakePHP's generated output.

By default, the following simple method:

    <?php echo $this->Form->input('username'); ?>
    

will output:

    <div class="input text required">
        <label for="UserUsername">First Name</label>
        <input type="text" id="UserUsername" maxlength="20" name="data[User][username]">
    </div>
    

I was struggling to figure out how to achieve the following output:

    <div class="input text required">
        <label for="UserUsername">Username</label>
        <input type="text" id="UserUsername" maxlength="20" name="data[User][username]">
        <p class="guidance">Must be the same as your AD Account</p>
    </div>
    

Then I found it. The Form Helper input method accepts options of `before`, `after`, `separator`, etc. I had originally assumed options were only for the automagic date fields. But sure enough, when tried the following, I achieved my desired output:

    <?php echo $this->Form->input('username', array('after' => '<p class="guidance">Must be the same as your AD Account</p>')); ?>
    

I look forward to the evolution of options passed into the Form Helper. In this specific case, personally I would have felt better about setting a `guidance` option rather than the `after`. Maybe with the adoption of HTML5, CakePHP may very well offer such an option. Until then, I hope this helps.

 [1]: http://www.alistapart.com/articles/makingcompactformsmoreaccessible/
 [2]: http://book.cakephp.org/#!/view/1383/Form
