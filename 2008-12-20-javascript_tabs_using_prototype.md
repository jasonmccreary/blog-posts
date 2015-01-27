---
title: JavaScript Tab Solution using Prototype
author: Jason McCreary
excerpt: >
  A JavaScript tab solution with a nice Scriptaculous Effect that uses semantic
  markup, CSS, and degrades gracefully.
layout: post
comments: true
permalink: /2008/12/javascript_tabs_using_prototype/
tags:
  - front-end
  - javascript
  - prototype
  - scriptaculous
  - UI
---
I make a best effort to write valid XHTML and use the [W3C Validator][1] throughout development. So, when I needed a tab solution, I knew exactly where to look. The W3C Validator consists of 3 forms with several form options. They bring these together very intricately on a single page with a custom tab solution. But I required something a bit more general. Furthermore, their solution used MooTools. Nothing against MooTools – or other JavaScript Frameworks for that matter – I just use Prototype. So, yet again, I wrote a port.

## Tab Requirements

For me, a tab solution should meet the following requirements:

*   The XHTML is valid, semantic, and clean
*   JavaScript is added progressively, and therefore should degrade gracefully
*   Configurable options

## Their Solution

As I said before, the W3C&rsquo;s solution was a bit specific. The containing elements where `<fieldsets>` within a `<form>`. I converted these to a `<div>` wrapped within a `<div>`. It may border "divitis", but it affords greater flexibility.

Their solution did an excellent job of adding the JavaScript progressively. With JavaScript disabled, it degrades to stacking the tabbed content. The tabs still function as navigation. This comes together elegantly by a named anchor. Many of the solutions I reviewed had obtrusive JavaScript with `onclick` attributes or `href="#"`. So credit to the W3C developers on this.

As a small note, their solution manipulated the hash (named anchor link) with JavaScript. Maybe a requirement for their needs, but it didn&rsquo;t seem necessary. In addition, with JavaScript disabled it breaks page links containing the hash. Very small, but it can function without manipulating the hash.

Finally, there were not configuration options. At a minimum it should have the following options:

*   Multiple tab solutions on a page
*   Equalizing the height for the tab content for a consistent look
*   Changing the "toggle" event for the tabs
*   Rounding the content container

## The Prototype Port

So here is [an example][2] of the tab solution or you can [download the source][3]. I will discuss the XHTML, CSS, and JavaScript below. However, I recommend looking thru the example first.

### The XHTML

The markup requires three placeholders. A containing element with an `id` attribute referenced by JavaScript and `class` attribute of `tabset`. A toggle element with a `class` attribute of `tabset_toggle`. I use a `<ul>` for semantic reasons and style the `<li>` and anchor tags to make the tabs. You can wrap the anchor within other tags, such as a heading tag for SEO. Finally, a content element with a `class` attribute of `tabset_content`. Again, I use a `<div>` for flexibility. Tabs are paired with content in order, so the first tab links to the first content `<div>`. I could have paired them by `id` with the named anchor. However, that would require code which I felt unnecessary given the content should always follow in the same order as the tabs.

    <div id="tabs1" class="tabset">
        <ul class="tabset_tabs">
            <li class="active"><a href="#tab-one">Tab One</a></li>
            <li><a href="#tab-two">Tab Two</a></li>
            <li><a href="#tab-three">Tab Three</a></li>
        </ul>
        <div class="tabset_content_container">
            <div id="tab-one" class="tabset_content">
                <h2>Tab One</h2>
                <p>Content goes here....</p>
            </div>
            <div id="tab-two" class="tabset_content">
                <h2>Tab Two</h2>
                <p>Content goes here....</p>
            </div>
            <div id="tab-three" class="tabset_content">
                <h2>Tab Three</h2>
                <p>Content goes here....</p>
            </div>
        </div>
    </div>
    

### The JavaScript

*tabs.js* contains a class extension using Prototype that allows you to create Tab objects. You can create a new Tab with the following code:

    document.observe('dom:loaded', function() {
        new Tab({id: "tabs1", rounded: 1, height: 1});
    });
    

This will setup the behavior and add any markup progressively based on the options provided. Currently, there are three: `id`, `rounded`, and `height`. `id` is required and must match the `id` attribute of the containing element with markup above. `rounded` is optional. If set to a true value, by JavaScript convention, it will create markup to give the content area rounded corners in the top right and bottom. Finally, `height` is optional. If set to a true value, it will set all tab content areas to the height of the tallest content area.

If multiple tabs exist on a page the explicit creation of a `new Tab` seemed redundant. Furthermore, if you use a CMS or work on a team, adding JavaScript may not be an option. The follow code leverages the existing `tabset` class and could be added to a JavaScript init script.

    function prepareTabs() {
        $$('.tabset').each(function(e) {
            new Tab({id: e, rounded: 1, height: 1});
        });
    }
    

As it is now, each `tabset` shares the same configuration. It would be difficult to adjust for individual tabs. One way around this would be to create more `tabset` classes to represent different configurations. However, with more than few options, the permutations add up.

### The CSS

As with most of my UI projects, the CSS is the trickiest part. However, by shifting all design responsibility to CSS I can style this to fit any design. There are a few areas of note. The tabs images use the [sliding door technique][4]. The content areas fade in by setting the `opacity` and toggling between `display: none` and `display: block`. If configured with rounded corners, JavaScript will add `<div>`‘s to the top and bottom of the content area with classes of `tr`, `bl`, and `br`. There are a few special styles for IE. All of which are commented. Most deal with hasLayout. I also had to add a background to the elements I noticed a [ghosting bold][5] during the effect. I hope to remove these in time. Finally, be aware of box model browser inconsistencies when adjusting the height, width, padding of your tab elements. They caused the most headaches.

## In Closing

Tab solutions are common and should be simple to use. They are a great way to group content and maximize page real-estate. This solution works very well in that respect. It provides several configurable options, fits any design, uses valid/semantic XHTML, and degrades gracefully.

**Note:** You could use these tabs for site navigation by splitting the content across other pages and transfer responsibility of some JavaScript code to a back-end language. Feel free to [post a comment][6] or [contact me][7] for more details.

 [1]: http://validator.w3.org
 [2]: /examples/tabs/
 [3]: /examples/tabs/tabs.zip
 [4]: http://www.alistapart.com/articles/slidingdoors/
 [5]: http://github.com/madrobby/scriptaculous/wikis/effect-fade
 [6]: #cform
 [7]: /contact
