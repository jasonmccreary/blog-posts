---
title: "Review of PHPMyAdmin 3.4"
heading: "Review of PHPMyAdmin 3.4"
author: Jason McCreary
excerpt: "PHPMyAdmin 3.4 came with many UI changes. After a few months of using PHPMyAdmin 3.4, I'm weighing the pros and cons."
subheading: "PHPMyAdmin 3.4 came with many UI changes. After a few months of using PHPMyAdmin 3.4, I'm weighing the pros and cons."
layout: post
comments: true
permalink: /2011/10/review-phpmyadmin-3-4/
categories:
  - Main Thread
tags:
  - review
description: PHPMyAdmin 3.4 came with many UI changes. After a few months of using PHPMyAdmin 3.4, I'm weighing the pros and cons.
---
A few month ago I needed to install [PHPMyAdmin][1] on my new Macbook Pro. At that time, PHPMyAdmin 3.4.3 just released. After installing PHPMyAdmin 3.4.3, I immediately noticed the theme and user interface changed. At first, I welcomed the change. The new theme, *pmahomme*, was clean and modern. So dramatic that someone asked what I used during a demo in [my talk at WordCamp Chicago][2].

Unfortunately the euphoria was short lived. The user interface changes soon outweighed the new theme. With due respect to the &ldquo;reluctance to change&rdquo; and &ldquo;learning curves&rdquo;, I question some of the decisions made in PHPMyAdmin 3.4. Here's my quick list the UI *pros* and *cons* of PHPMyAdmin 3.4.3. Let me disclaim that I use PHPMyAdmin as a development tool.

## PHPMYAdmin 3.4 Pros

*   **Faster response times.** In addition to undoubted PHP optimizations, the combinations of jQuery and CSS make the UI much faster.
*   ***Inline* query editing.** While the pop-up window works, this UI is much more streamlined. Probably the single best feature.
*   ***Create table* button in table list.** Before you had to go to the database home page (or *Operations*), scroll down, and add a table. PHPMyAdmin 3.4 could go further, having this button at the top and bottom (for large table lists).

## PHPMYAdmin 3.4 Cons

*   **Removal of *Empty* and *Drop* tabs from table navigation.** While I appreciate these are not technically navigation options, burying them under *Operations* was not the right move. If nothing else, taking features *away* is always a dangerous game. I have to say PHPMyAdmin lost me on this one. As a developer, these actions, particularly *Empty* are incredibly common.
*   **Hiding of *Export* options.** Instead of displaying all options, like previous versions, the UI is now that of a decision tree. In addition, the options themselves changed. Again, this is an often used feature for me. I could export unconsciously in previous versions. It seems, even after months, I'm continually scanning the options in PHPMyAdmin 3.4.

All in all, it appears that PHPMyAdmin 3.4 caters to new users. I suppose you can't fault them for appealing to the masses. Yet for my usage the cons of PHPMyAdmin 3.4 outweigh the pros. Although I'll probably stick with it on my personal server, I have left PHPMyAdmin 3.3 on our work servers. Those extra clicks spread across 4 developers add up. As far as the new features, well, ignorance is bliss.

 [1]: http://www.phpmyadmin.net/
 [2]: http://jason.pureconcepts.net/2011/07/wordcamp-chicago-talk-configuring-multiple-environments-wordpress/ "My Talk at WordCamp Chicago: Configuring WordPress for Multiple Environments"
