---
layout: post
title: "PHP Namespaces: Avoid use"
heading: "PHP Namespaces: Avoid use"
date: 2013-04-01 22:46
comments: true
categories:
  - Main Thread
excerpt: "A look at how <code>use</code> breaks PHP namespaces and why you should avoid using <code>use</code>."
subheading: "A look at how <code>use</code> breaks PHP namespaces and why you should avoid using <code>use</code>."
description: A look at how use breaks PHP namespaces and why you should avoid using use.
---
`use` breaks the fundamental aspects of PHP namespaces. Avoid `use`.

Oh, you want to know *why*? Fine. Keep reading.

Let's review the fundamental aspects of PHP namespaces as stated in the [PHP Docs Namespace Overview](http://www.php.net/manual/en/language.namespaces.rationale.php):

> namespaces are designed to solve two problems that authors of libraries and applications encounter when creating re-usable code elements

So namespaces focus on  *re-usable code elements*. Let's look at the two problems:

> 1. Name collisions between code you create, and internal PHP classes/functions/constants or third-party classes/functions/constants.
> 2. Ability to alias (or shorten) Extra_Long_Names designed to alleviate the first problem, improving readability of source code.

`namespace` solves problem #1. `use` solves problem #2. But things can get circular. `use` can introduce name collisions and confusion. Which respectively reintroduces problem #1 and is the opposite of *improving readability*.

Consider the following namespaces and classes:

    // MyApp/Service.php
    namespace MyApp;
    
    class Service {
        public function method() {
            echo __NAMESPACE__ . '\Service';
        }
    }

    // MyApp/ComponentA/Service.php
    namespace MyApp\ComponentA;
    
    class Service {
        public function method() {
            echo __NAMESPACE__ . '\Service';
        }
    }

A `Controller` class with `use`:

    // MyApp/Controller.php
    namespace MyApp;
    
    use MyApp\ComponentA\Service;
    
    class Controller {
        public function output() {
            $service = new Service();
            $service->method();
        }
    }

Which `Service` class is created? `MyApp\Service` or `MyApp\ComponentA\Service`?

It may be straightfoward with the entire codebase fits within your screen. But consider a larger codebase. What if you refactored `output()` into another class? It all depends on the `use` statement. Meaning the code is tightly coupled with `use`.

The same `Controller` class without `use`:

    // MyApp/Controller.php
    namespace MyApp;

    class Controller {
      public function output() {
        $service = new MyApp\ComponentA\Service();
        $service->method();
      }
    }

No question on which `Service` class is created and no coupling.

There are other problems with `use`, such as dynamic naming. But you don't need more convincing. `use` breaks what namespaces solve. 

Spend a few extra keystrokes typing absolute namespaces for code clarity and portability. Future developers will thank you.
