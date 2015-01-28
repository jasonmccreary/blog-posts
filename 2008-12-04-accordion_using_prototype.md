---
title: "JavaScript Accordion using Prototype"
heading: "JavaScript Accordion using Prototype"
author: Jason McCreary
excerpt: "So there are several accordion solutions out there. But after reviewing a few, I created this hybrid using lean markup, simple CSS, and the Prototype and Scriptaculous libraries which degrades better than its predecessors."
subheading: "So there are several accordion solutions out there. But after reviewing a few, I created this hybrid using lean markup, simple CSS, and the Prototype and Scriptaculous libraries which degrades better than its predecessors."
layout: post
comments: true
permalink: /2008/12/accordion_using_prototype/
tags:
  - front-end
  - javascript
  - prototype
  - scriptaculous
  - UI
---
In my search for an accordion solution I came across two of mention. The first was from [stickmanlabs][1]. It had good functionality and the ability to create a horizontal accordion. But support for IE6 was lacking (although damn IE6) and and it used an older version of Prototype. I kept looking and came across the second, by [Brian Crescimanno][2]. This was a little smoother, with leaner markup. The code was cleaner and appeared very similar to stickmanlabs. So, I felt like I was on the right track. After reviewing the demo and reading through the comments, it needed more. So, in typical developer fashion, I made my own.

## The Merge

In this case, I felt there were pieces of both solutions that were good. I decided to merge the two, and use Brian Crescimanno&rsquo;s as a base. If you want more detail on the individual solutions, I suggest reviewing the articles above. As such, I have provided an outline of the changes:

*   Updated stickman&rsquo;s horizontal functionality to Prototype 1.6.
*   Made more *prototype-ish*. Code wasn&rsquo;t taking full advantage of Prototype.
*   Refactored methods by grouping similar actions (e.g. toggle/clickHandler)
*   Removed modification of `display` styles, used `height`/`width` consistently.
*   Converted `initialize` parameter into an options hash.
*   Ability to change the "toggle" event.
*   Ability for multiple nodes to be expanded in a vertical accordion. Neither solution had this, and in my opinion it mimics a true accordion.
*   Better degradation. Modified "toggle" elements to be anchor tags. Integrated CSS styles to allow proper display if JavaScript is disabled.

## The Solution

So without farther ado, here is [an example][3] of the merged accordion solution or you can [download the source][4].

The markup requires three placeholders. A containing element with an `id` attribute you reference with JavaScript. A toggle element with a `class` attribute of `accordion-toggle`. I use an anchor tag for semantic reasons, but it can be anything. A content element with a `class` attribute of `accordion-content`. There is a one to one relationship between toggle and content elements.

    <div id="test-accordion">
        <a href="#" class="accordion-toggle">Main</a>
        <div class="accordion-content">
            <p>Lorem ipsum dolor sit amet, consectetuer adipiscing elit.</p>
            <p>Mauris dictum congue lectus.</p>
        </div>
    

Currently the configuration options are the accordion type and event. Type can be `horizontal`, `vertical` (default), or `vertical-multiple`. Event can be any event supported by Prototype (e.g. mouseover, click). In addition, you can change the class names within the `initialize` method. However, keep in mind these are shared for all your accordions. The following code from the example creates a vertical and horizontal accordion on page load:

    document.observe("dom:loaded", function() {
        accordion = new Accordion({id: "test-accordion"});
        accordion2 = new Accordion({id: "test2-accordion", type: 'horizontal'});
    });
    

The CSS is the trickest part. In modifying the CSS, I found that most bugs were related to the styles. When styling your accordion remember [the box model][5]. Padding, margin, and borders all affect the effect. Which makes sense considering this solution modifies `height`/`width`. If you start noticing jumpiness in the effect, check these properties.

## In Closing

An accordion solution is relatively progressive. Although I feel this solution degrades better than others, it is not fully functional without JavaScript enabled. To resolve this, you will need some back-end support. By modifying the toggle elements to link to the current page with a URL parameter. On page load the back-end could parse this URL parameter to identify which node to expand and add the necessary CSS class (`active`).

### A Disclaiming Note

Understand the web is a continually evolving environment. The code within this article is offered with an as-is warranty. My goal is that the article may help more than the code. Nonetheless, I always welcome your feedback, good and bad. Just know, that I know, this is not *the solution* and therefore may not work for you… Although in an [ideal development world][6] it would.

 [1]: http://www.stickmanlabs.com/accordion/
 [2]: http://nettuts.com/javascript-ajax/create-a-simple-intelligent-accordion-effect-using-prototype-and-scriptaculous/
 [3]: /examples/accordion/
 [4]: /examples/accordion/accordion.zip
 [5]: http://www.w3.org/TR/CSS2/box.html
 [6]: /articles/fallout_of_babel
