---
layout: post
title: Forward-Compatible Code
date: 2013-02-05 22:01
comments: true
categories:
  - Main Thread
excerpt: While updating some legacy code to be backward-compatible I found that forward-compatibility was equally important.
description: While updating some legacy code to be backward-compatible I found that forward-compatibility was equally important.
---
Most developers know about backward-compatibility &mdash; ensuring code runs under past versions. But what about forward-compatibility? Developing code compatible with future versions can be equally important.

I recently had a task to update some legacy code. This code was high-profile. All of our stacks ran a version. So I needed to ensure that my updates were compatible.

Upon reviewing the code it contained a version variable (`VER`). Perfect! A version variable is perfect to use as a [feature flag](http://code.flickr.net/2009/12/02/flipping-out/ "Feature Flags"). I could wrap my updated code inside conditionals until the rest of the stacks were upgraded.

    if (thisver == 101) {
        // new code
    }

Done. Right? Well, yes and no. *Yes*, this code is backward-compatible. I safely wrapped my new code in featured flags. So I can rest assured it will only run when the feature flag is set. In this case, when `thisversion` equals `101`. However, *no* because unit tests failed for old versions of the code.

After some debugging, I found the issue:

    if (thisver > VER || thisver < 100) {
        return;
    }

Although I incremented `VER` for my new version, the old version still had the previous version number. While my code was indeed backward-compatible, old versions always failed this little gem: `thisver > VER`. The old code was not *forward-compatible*. This logic prevented me from using an ideal version variable as a feature flag.

I will not speculate on the original developer's intention. However, given the code's high-profile and the existence of `VER` the logic above is *odd* and a good demonstration of code that is not forward-compatible.
