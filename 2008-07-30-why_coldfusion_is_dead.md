---
title: "Why ColdFusion is Dead"
heading: "Why ColdFusion is Dead"
author: Jason McCreary
excerpt: "An explanation on why I never adopted ColdFusion. Including a deeper look at several features, or lack thereof, that drive me nuts and some suggestions on how things could turn around."
subheading: "An explanation on why I never adopted ColdFusion. Including a deeper look at several features, or lack thereof, that drive me nuts and some suggestions on how things could turn around."
layout: post
comments: true
permalink: /2008/07/why_coldfusion_is_dead/
categories:
  - Main Thread
tags:
  - coldfusion
  - philosophy
  - rant
description: An explanation on why I never adopted ColdFusion. Including a deeper look at lacking features and suggestions for language improvements.
---
ColdFusion is a tag-based, back-end web language that has been around since the mid-1990′s. I first used ColdFusion on a client site several years ago. For me, it was the simplest language I every learned because of its similarity with another language I used at the time, iHTML. They seemed to share the exact same tag set, aside from the prefix. `<itag>` here was `<cftag>` there. I used ColdFusion again for a client a few years later, now version 6 (MX). Not much had changed, which allowed me to get the job done quickly. A developer normally adopts using a language for such reasons. Yet, I didn't. Even as a developer with a ColdFusion shop for 3 years, I never personally adopted the language.

Why? The answer is simple, in my experience of the language, across several versions, it has not evolved. Maybe the language's change of hands, Allaire – Macromedia – Adobe, put it behind the times. Understandable. Yet, I feel that even the languages core features left wanting.

### Dated Practices

As I said before ColdFusion is a tag-based language. This makes the language simple and straightforward. But you immediately lose the efficiency of developing complex code with a simple script. `<cfscript>` the evangelists scream. `<cfscript>` made ground in ColdFusion 8 (I will come back to this). Yet there is something fundamentally limiting about `<cfscript>`. It doesn't support ColdFusion tags, you can only use ColdFusion functions within a `<cfscript>` block. Wait, that means within a `<cfscript>` block you lose most of this tag-based language's features. Opps!

Not to harp on the limited scripting of ColdFusion, but their lack of basic operators. As I referenced above, it wasn't until ColdFusion 8 that the language added support for post-increment and overloaded assignment operators. Before that if I wanted to increment a variable I had to write:

    <cfset Variables.SomeValue = Variables.SomeValue + 1>
    <cfset Variables.SomeValue = IncrementValue(Variables.SomeValue)>
    

Finally, after version 8.

    <cfset Variables.SomeValue += 1>
    <cfset Variables.SomeValue++>
    

I mean, even QBASIC had these operators. Another example of how behind ColdFusion is compared to other languages.

In my opinion, tag-based back-end languages are a thing of the past. Most tag-based languages are used for simple markup on the front-end (e.g. HTML, XML). They thrive on the front-end, but fall short on the back-end. They are unweildy in that capacity. I believe ColdFusion has reached that critical mass. Developer's require more elegant solutions, and ColdFusion is not up to the challenge.

This leads me to my second big complaint of ColdFusion, Not Object Oriented. ColdFusion does have the concept of a `<cfobject>` created from a `<cfcomponent>` (among others). These are primitive stages and only recently was support for core OO principles such as inheritance added. OOP has been around since the early 1970′s and has taken off on the web with the rise of MVC (also from the 1970′s). It's sad to think that a language, even by version 8, doesn't support OOP these days. Unfortunately, ColdFusion isn't the only one. 

ColdFusion is the limited array support also stiffles me. Admittedly this is not as strong a point as the others. Nonetheless, it fits the bill. Array's are a basic data structure of all launguages. Although ColdFusion does have arrays, they are not used by core features. ColdFusion uses Lists instead. A ColdFusion List, essentially, stores a delimited string. Most functions and tags accept or return lists. Whenever ColdFusion abandoned arrays and went their own way with lists they made a mistake. They turned their back on developers using a fundamental data structure.

### Inconsistent Behavior

Every language deals with consistency issues. The order of arguments for a List function differs from String function. ColdFusion is no different. But there is a specific set of display features in ColdFusion that in all my experience I can't explain. I have submitted bug reports, emailed user groups, asked speakers at Adobe Max and [CFObjective][1]. Nothing. I open to the possibility that maybe some else is wrong in my code. But one would like to think that after all the steps above such an explanation would have been provided.

#### cfoutput and cfloop

Output in ColdFusion has always been a finicky beast. The amount of whitespace left from the original markup is annoying in itself. Configuration options exist, but it's really a choice between the lesser of two evils. I suppose you could litter your code with `<cfoutput>` only where you needed dynamic data. But that seems pretty tedious. Whitespace aside, `<cfoutput>` is an also finicky. It is a tag that you can or can't nest depending on the data you wish to output. If you add the `group` attribute, well, good luck. But the one that takes the cake is a combination of `<cfoutput>`, `<cfloop>`, and `<cfquery>`

#### cfdocument

`<cfdocument>` is the nail in the coffin for ColdFusion. As much potential as this tag has, to output content to PDF/Flash Paper, it is horrible. For me, this single tag embodies the entire language. ColdFusion Team developers have openly cursed this tag in talks and screencasts. I know any developer that has attempted to use this tag for even a slightly complex task agrees with me. As such, I won't waste time explaining all bugs. Since the existence of this tag, there are 3 issues that no amount of configuration, markup, upgrades, bug report, or development insight has solved.

*   Caching issues. Even with a fresh cache and URL GUID stale content will output for more complex `<cfdocument>` tasks. A refresh displays the latest.
*   Headers and Footers do not work according to ColdFusion 8 Spec, even when using `<cfdocumentsection>`. Instead the last set of headers/footers overwrite all.
*   At normal zoom, the font is randomly larger/small than the specified size when outputting even the simplest HTML in PDF format. Tested in several browsers and platforms.

### A New Hope

ColdFusion really could turn around. Yes, I have been complaining about it for several paragraphs. Indeed, I feel they are all good points. Yet, I give ColdFusion due credit in a few areas.

*   Easy to learn, easy to use
*   Straightforward error handling
*   The concept of an Application scope is intuitive
*   Native PDF support
*   Recent increases in the ability to leverage Java directly in you ColdFusion code.

These last two items are where ColdFusion could really shine and overcome a majority of the issues I mentioned above.

Java is an object oriented language. There is no excuse for ColdFusion not to leverage or expose this native support. After all, "ColdFusion *is* Java". At least that is what I keep hearing in conference talk directly from the CF Team. So I should be able to write `Firstname.substr()` or access a custom Java class. You can do these things now, but you have to instantiate a Java object or package you Java classes. Make it seamless. Make `<cfjava>` It may be messy, but allow some kind of hybrid interface.

ColdFusion has supported native PDF generation for some time now. Albeit buggy as mentioned above. But this is a really nice feature for a web language to have out of the box. When Adobe bought Macromedia, I had high hopes for features like Flash integration or image manipulation. I thought at least the `<cfdocument>` issues would be fixed. No, they spend time on tags like `<cfwindow>`, `<cfajaximport>`, etc. Why is a back-end language wasting time on front-end technology? In my opinion, this was a silly attempt to bring a dated language up to speed by integrating some of the latest web trends. `<cfpdf>` did come out. But it primarily manages existing documents. Adobe has a great product line, so let's see some synergy between products. Please though, now under Adobe at least fix features involving their trademark PDF.

### Closing

Until ColdFusion addresses the items above, they don't stand a chance at evolving. ColdFusion will become exactly that **cold**, as in *dead*. They could be an enterprise web language. Maybe not a contender with .NET, but most organizations still haven't taken that leap to open-source technologies. For whatever reason, they don't view them as *grown up* or *supported*. With ColdFusion now under Adobe's product line, that could provide an edge. And that is exactly it. If they took full advantage of their partnerships with Adobe and Java, the language would have much more potential.

 [1]: http://cfobjective.com
