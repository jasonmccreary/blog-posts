---
title: 'iPhone App: LastPlayed'
author: Jason McCreary
excerpt: 'A description of my recent iPhone App - LastPlayed. Including development notes, marketing strategy, future direction, and an invitation for your feedback. Complete with a shameless plug for you to download the app for free.'
layout: post
comments: true
permalink: /2009/11/lastplayed_iphone_app/
tags:
  - development
  - iphone
---
I started iPhone development in early 2009. I will admit it was a slow start. Even armed with several of the latest iPhone and Objective-C books, the learning curve was steeper than expected. I hit a lot of walls, with no where to turn. I decided to attend the 2009 WWDC, in San Francisco. Which, like anything from Apple, was expensive, but a great investment. I left with more confidence and additional app ideas on my constantly changing list. However, with new features available in the iPhone SDK 3.0, I wanted to ensure my next app would incorporate as many as possible.

## LastPlayed and LastPlayed+

LastPlayed combines access to iPod Library with in-app Google Maps, new to iPhone SDK 3.0. It&rsquo;s centered around answer a common question – "What are you listening to?" It does so by broadcasting your currently playing song across the LastPlayed Network. This is visible to other users when they first open the app or are in your proximity, and when browsing the map view.

There is also a *plus* version, for 99 cents, that adds some *valuable* features. First are integrated player controls. This keeps you from having to bounce between the iPod app and LastPlayed, and I spent extra time ensuring that the controls function just like the iPod. Second are some social networking features that allow users to share their song info via Facebook and Twitter. Sharing happens automatically for each song or manually depending on your settings.

For screenshots and a more detailed description check out both [LastPlayed][1] and [LastPlayed+][2].

## Developing LastPlayed

LastPlayed had two pain points during development. Surprisingly, accessing the iPod Library and Google Maps were not painful. Between the WWDC sessions and developer reference, they were relatively easy. My walls were hit when developing the interaction with the LastPlayed Network and placing pin on the integrated Google Map View.

### iPhone Apps and Web Services with PLists

Parsing XML seems to be the bane of any programming language. Most web languages have either added native support or offer some library extension. Objective-C has both a native, NSXMLParser, and library, xmllib2, solution. However, these are rather heavy and as such can make the learning curve just a bit steeper.

I spent some time researching XML solutions before deciding. For this app, I had a small dataset but a high transfer rate. So I wanted something really lightweight, both from a size and development perspective. After countless web searches and reading dozens of docs, I decided on using PLists. In doing so I realized something: Apple loves PLists. PList is a simple XML format, that takes about 6 minutes to learn. Since I controlled the web service, I just had the server output this format. Objective-C has several PList methods for reading, storing, and converting. Once I downloaded the data from the server, I had the PList parsed into a dictionary with 2 lines of code. **2 lines**. If you have a small dataset and control the source, I suggest trying PList.

### Placing a pin on Map View

I drop pin annotations on the map to represent where a song is playing. For scalability, I needed to determine if a pin already existed on the map so the new pin would not overlap. Both a WWDC session and the docs lead me to believe that this should be easy to determine. Somewhere, either by my misunderstanding or incorrect documentation, something went wrong.

In theory, I could simply test the occupied rectangle for the existing pin against the new pin. Yet, in practice, this didn&rsquo;t hold true. If I added a new pin to the same map coordinate the tip of the pin appeared at the upper left of the existing pin. Shouldn&rsquo;t it have put in the same place as the existing pin? There is a centerOffset property, which I expected to return an offset to compensate for such behavior. But it did not.

I battled with this for about a day. In the end, I offset the new pin by half the size of the existing pin. This gave me the expected results. I will not go so far to call this a bug, but at the least, it was unintuitive.

## Marketing LastPlayed

I felt the idea of LastPlayed was novel. I did my best to rush it to market. But as I often find with the App Store, although there was no competition during development, others existed by release. It&rsquo;s such a coincidence, it makes me wonder if Apple holds similar apps and releases them simultaneously to create competition. But, although biased, LastPlayed has much better features and interface. 

Regardless of features and first to market, getting an app into the App Store is a two fold marketing nightmare. First, there&rsquo;s really no way to know exactly when your app will be approved. So it&rsquo;s very difficult to coordinate any kind of initial release. Second, with 100,000 apps and counting, your app drowns within 2 days. Unless you have the best app ever or backed by a company budget, it&rsquo;s tough to stay on top.

All us garage developers are left with is:

*   Emailing family and friends
*   Leveraging social networks
*   Purchasing ads on Google, Yahoo, Facebook

Unfortunately, these are closed looped and there is no guarantee the audience has an iPhone or iPod Touch. Personally, I find ads to be a waste of money. Not only for the reason above, but ROI is minimal, possibly negative, and the metrics are poor. The latter is especially true with Facebook.

The good news is LastPlayed+ was built to market itself. Users more than likely buy the app to share what their listening to with friends on Facebook and followers on Twitter. Therein lies the rub. As LastPlayed+ grows, so does the amount of people that see it. At least that&rsquo;s the theory.

## Future Versions of LastPlayed

In order to get the app to market, the vision for LastPlayed was scaled back to a quarter of what is was. There are so many features I want to see in future version of LastPlayed. I am leery to publish them in this article. Nonetheless, the next update will include the ability to control the shared message and participate in #MusicMondays on Twitter.

I also have the intention of having LastPlayed+ features slowly filter down to LastPlayed. Of course not all features will end up in the free version. That would just piss people off. But don&rsquo;t be surprised to see player controls in the next upgrade of the free version.

Right now, it&rsquo;s difficult to decide the exact direction. Are the social networking features more appealing or the combination of player controls? In addition, with Apple now allowing In-App Purchase for Free Apps, LastPlayed and LastPlayed+ could soon merge.

## In Closing

LastPlayed has place in the App Store. It&rsquo;s a great idea and suits a music device like the iPod Touch and iPhone. I am proud of this app and can only hope to get the chance to continue adding features to see what the future brings.

I value your feedback. At the moment, I still have several promo codes remaining to get LastPlayed+ for free. First come, first served.

 [1]: http://iphone.pureconcepts.net/app/lastplayed/
 [2]: http://iphone.pureconcepts.net/app/lastplayedplus/
