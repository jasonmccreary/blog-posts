---
title: CakePHP Auth Component loginRedirect Behavior
author: Jason McCreary
excerpt: "I came across some interesting behavior with CakePHP's Auth Component. The solution didn't align with the documentation or my understanding of the Auth Component's login redirect. This post covers the problem and my solution. "
layout: post
comments: true
permalink: /2011/10/cakephp-auth-component-login-redirect/
categories:
  - Main Thread
tags:
  - cakephp
description: A post about some interesting behavior with login redirection in CakePHP's Auth Component and the unintuitive solution.
---
The other day I noticed some unexpected behavior for one of the web applications I developed in [CakePHP][1] using the [Auth Component][2]. After logging in, I was redirected to my previously *visited* page. This would be expected had my session expired or I needed to log in first. However, this occurred after logging out of the web application.

After debugging the `login()` and `logout()` actions I noticed that `Auth.redirect` was not being cleared from the session. I inspected the core file for the Auth Component (auth.php) to see when `Auth.redirect` updated. It turns out that the `startup()` method had some interesting code.

    
    if ($loginAction == $url) {
      // ...
      if (!$this->Session->check('Auth.redirect') && !$this->loginRedirect && env('HTTP_REFERER')) {
        $this->Session->write('Auth.redirect', $controller->referer(null, true));
      }
    

The interesting piece is the inclusion of `$this->loginRedirect`. According to the documentation:

> The AuthComponent remembers what controller/action pair you were trying to get to before you were asked to authenticate yourself by storing this value in the Session, under the `Auth.redirect` key. However, if this session value is not set (if you&rsquo;re coming to the login page from an external link, for example), then the user will be redirected to the URL specified in `loginRedirect`.

Yet, according to the code above, `$loginAction` *sets* `Auth.redirect` *unless* it or `loginRedirect` is set. I added the following code to my Auth Component configuration:

    'Auth' => array('autoRedirect' => false, 'loginRedirect' => '/', &hellip;

After doing so, I expected that the Auth Component would no longer remember the requested URL if I was not logged in. But, contrary to the documentation, I was still redirected when attempting to access a secure area of the site before logging in.

Honestly, my head exploded on this one. I&rsquo;m still a little fuzzy on `loginRedirect`. But here&rsquo;s my two cents. The original issue only occurred after logout. Since logout redirects to `loginAction`, the referrer was the previously requested page. Although `logout()` cleared `Auth.redirect`, the code above stored the referrer in `Auth.redirect`. Upon setting `loginRedirect`, the logic above failed. Since this code only runs when `$loginAction == $url`, it does not prevent Auth from remembering the requested URL when it *matters*.

I find many developers dislike the Auth Component. Whether it&rsquo;s too complex or not enough features, I don&rsquo;t know. What I do know is there is value in using native functionality. So, to be clear, this post is about the unclear documentation for the `loginRedirect` property and not bashing the Auth Component.

 [1]: http://cakephp.org/ "CakePHP"
 [2]: http://book.cakephp.org/view/1250/Authentication "CakePHP Auth Component"
