---
title: "Conditional Validation in CakePHP"
heading: "Conditional Validation in CakePHP"
author: Jason McCreary
excerpt: "I needed to perform conditional data validation in CakePHP for a project recently. This is a review of the internal options to do so and my own approach."
subheading: "I needed to perform conditional data validation in CakePHP for a project recently. This is a review of the internal options to do so and my own approach."
layout: post
comments: true
permalink: /2011/03/conditional-validation-cakephp/
categories:
  - Main Thread
tags:
  - cakephp
  - development
description: I needed to perform conditional validation in CakePHP. This is a review of the internal options available and my own solution.
keywords: cakephp, validation, conditional, model
---
I&rsquo;ve been using CakePHP as the framework for an Intranet project. Although new to CakePHP, I knew that for a long term project an MVC, rapid development framework provided a strong foundation. That has proven itself time and again in the last 9 months. Sometimes though, determining how to do something within a framework can be difficult.

The other day, I had a requirement for one of the applications for conditional data validation. CakePHP provides flexible and simple validation of your Model data that combined forms very complex rules. Unfortunately, nothing out of the box handled conditional validation.

To be fair, there are some native options that may work for you in *simple* cases. You could add `'allowEmpty' => true` to your rules. Yet, setting anything at the Model rule level applies across all Controllers. Conditional validation validates the Model one way in Controller A, but differently in Controller B. Yes, I know that&rsquo;s lame for data integrity. But we developers must support real world demands from clients. Anyway, there are also ways to [validate from the controller][1]. These looked promising. But in practice, the code quickly becomes bloated. You begin to have calls to `validate()` before every `save()`. Even more for multi-model `saveAll()` calls. You also have to list the Model fields to validate in the Controller. Too much coupling in my opinion. So this becomes a maintenance issue beyond a few isolated cases. Another alternative is create [custom validation rules][2]. Similar to the above though, code becomes bloated outside a few rules. In addition, you are now responsible for validating the data yourself and can&rsquo;t take advantage of CakePHP&rsquo;s core rules (e.g. email, ssn).

Before I began developing anything, I did a quick Google search. The only thing I found was an outdated Model Behavior. So I decided to make my own with the following goals:

*   I wanted it to be as native as possible. This meant keeping the core validation rules. I merely wanted to enable, disable, or modify validation rules on the fly.
*   I wanted to keep with [Fat Models, Skinny Controllers][3]. Meaning low coupling – always a plus. This way the controller simply tells the model *how* to validate from a high level. Not explicitly *what* to validate.

So here&rsquo;s what I came up with. Validation rules could be set by passing a condition to a Model method. All the logic to setup the appropriate validation is encapsulated within this method. The controller simply calls this function with the argument to perform conditional validation for that Model. An example call would be:

    $this->Employee->setValidationRules('it');
    ...
    $this->Employee->save($this-data);
    

If no conditions are passed, the Model would use the default validation rules. In order to set these automatically, I overrode the constructor method. The reason I put this here instead of leaving it as a Model property was to allow the Controller to *reset* the validation rules.

    function __construct($id = false, $table = null, $ds = null) {
        parent::__construct($id, $table, $ds);
    
        $this->setValidationRules();
    }
    
    function setValidationRules($condition = null) {
        if ($condition == 'it') {
            // turn off field requirements for nea_it users as they don't have this data yet
            unset($this->validate['computer_type']);
            unset($this->validate['computer_service_tag']);
            $this->validate['company_email'] => array('boolean' => array('rule' => array('email', 'allowEmpty' => true)));
        }
        else {
            // default validation rules
            $this->validate = array('rental_car_cards' => array('boolean' => array('rule' => array('boolean'))),
                'company_car' => array('boolean' => array('rule' => array('boolean'))),
                'company_car_allowance' => array('boolean' => array('rule' => array('boolean'))),
                'company_cell_phone' => array('boolean' => array('rule' => array('boolean'))),
                'company_cell_phone_plan' => array('dependent' => array('rule' => array('notempty'))),
                'computer' => array('boolean' => array('rule' => array('boolean'))),
                'computer_type' => array('dependent' => array('rule' => array('notempty'))),
                'computer_service_tag' => array('dependent' => array('rule' => array('notempty'))),
                'company_email' => array('boolean' => array('rule' => array('email'))));
        }
    }
    

It&rsquo;s primitive. But it does satisfy the client&rsquo;s spec and meets my goals. I think adding some callback hooks to reset the validation automatically could be helpful. Then conditional validation would behave much like `bindModel()` or `$this->Model->recursive` where it only effects the next call to the Model. Which is more *Cakeish*. Yet validating data on the same Model under two different conditions in the same request is probably very rare. In the end, I think it&rsquo;s a straightforward way to do conditional validation in CakePHP.

 [1]: http://book.cakephp.org/view/1029/read#!/view/1182/Validating-Data-from-the-Controller
 [2]: http://book.cakephp.org/view/1029/read#!/view/1179/Custom-Validation-Rules
 [3]: http://bitfluxx.com/2008/01/23/cakephp-best-practices-fat-models-and-skinny-controllers.html
