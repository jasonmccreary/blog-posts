---
title: "CSS Organizer and Minifier"
heading: "CSS Organizer and Minifier"
author: Jason McCreary
excerpt: "A specification for developing a functional CSS Organization and Minification tool that can be used to manage CSS files."
subheading: "A specification for developing a functional CSS Organization and Minification tool that can be used to manage CSS files."
layout: post
comments: true
permalink: /2008/10/css_organizer_and_minifier/
tags:
  - css
  - front-end
  - tools
---
### The Problem

During the development process I may have several CSS files containing all sorts of rules. These names may be based by page or the general rules contained in each file. However, in production, from both a maintainability and performance perspective, the last thing you want is several files. Ideally, you would like a single, compact CSS file for production. If usings themes, you may want more. So between development and production, you want two very different things.

### The Proposed Solution

Normally, when one needs to go from a raw material to a finished products, a tool is used. In this case, I need something that can process my CSS files between development and production. Maybe it can also do some additional items during processing, like code formatting, file organization, or minification. I did not find such a tool that existed. So I have undertaken this project to develop such a tool.

With any tool there are guidelines or a practice. You wouldn&rsquo;t use a chainsaw while building a dollhouse. So even though this will not fit the need, my hope is that this tool will nonetheless help.

#### Project Specification

I know we are all different, which makes this project near impossible. I have done the best to accommodate that into this spec. Nonetheless, with every tool there are guidelines. I mean you wouldn&rsquo;t use a chainsaw while building a dollhouse. My hope is that by adopting a few industry conventions, and drawing some lines, this tool will help a maximum audience.

*   Accept CSS files 
    *   files can be within directories
    *   ability to provide file cascade manually or automatically from convention
*   Process CSS files 
    *   categorize declarations into three groups: layout, typography, and style
    *   output categorized CSS into repectively named files
*   Miscellaneous 
    *   validation
    *   simple formatting options
    *   simple minification options: whitespace, shorthands, duplication

### In Closing

I have begun an alpha version following the above spec in Java. My goal is to create a proof of concept and use it during a project before releasing a web version on this site. However, you requests are **required**. So please, post comments or send me feedback directly.
