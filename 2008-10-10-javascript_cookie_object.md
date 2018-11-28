---
title: "JavaScript Cookie Object using Prototype"
heading: "JavaScript Cookie Object using Prototype"
author: Jason McCreary
excerpt: "A Cookie class in JavaScript, using Prototype, allowing multiple data objects to be stored in a single Cookie. Class contains methods for data management and storage."
subheading: "A Cookie class in JavaScript, using Prototype, allowing multiple data objects to be stored in a single Cookie. Class contains methods for data management and storage."
layout: post
comments: true
permalink: /2008/10/javascript_cookie_object/
tags:
  - front-end
  - javascript
  - prototype
---
I have been using [Prototype][1] for the last year. I am still relatively new, but so far it does everything I need it to do natively. I have build many reusable scripts that do everything from adding simple events to automatic front-end form validation. With additional visual effects by the partnering [Scriptaculous][2] library, there isn't much left. That is until I came across the need for front-end cookie management.

### Why Front-End Cookie Management

As a back-end programmer, front-end cookie management seems silly. Why would I need or want to use something like JavaScript to manage cookies. Until recently, I would have just used PHP or Ruby for the job. However, I have found myself on contract as Lead Front-End developer. As such, those technologies are not available to me. Furthermore, tasking someone down on the IT side of the house can be a time consuming hassle. And came you blame them? I'm able to reverse the roles, and if some marketing guy asked me to set up a cookie to store XYZ, I'd laugh. Rightly so too, why should I waste such time storing things like text size, toggled modules, etc on the back-end. After all, aren't such examples why cookies exist. User settings or preferences may necessitate back-end involvement, and these can be stored in a cookie for convenience. Yet, something dealing with the UI doesn't really warrant involvement.

### Where's the Cookie Monster

So the obvious choice to manage cookies on front-end is JavaScript. During research, I came across two interesting pages. The first was a collection of [Top 10 JavaScripts][3], with the top being cookie management functions ported from PHP. Second was a JavaScript class called [CookieJar][4]. At first, I didn't quite understand the point. See to JavaScript the cookie comes across as a simple key value pair as semi-colon separate string in `document.cookie`. If you want to track several variables, you would need to set as many cookies. That would get old… err stale. Anyway, instead of having all these cookies floating around, the JavaScript CookieJar organized then for you.

### Enter Prototype

CookieJar was a little primitive. The premise, to store the variables as a hash in a single cookie, was sound. But it didn't actually handle cookie storage. Now this may be kitchen for the Object Oriented elitists, but I merged the two scripts. To regain some ground, I wanted my Cookie class to be a Singleton Gateway. As such, it should do everything required to access, manage, and maintain the Cookie. Since I was already using Prototype, I took advantage of its Hash object. I ended up with the script below.

    var Cookie = {
      data: {},
      options: {expires: 1, domain: "", path: "", secure: false},
    
    init: function(options, data) {
      Cookie.options = Object.extend(Cookie.options, options || {});
    
      var payload = Cookie.retrieve();
            if(payload) {
                Cookie.data = payload.evalJSON();
            }
            else {
                Cookie.data = data || {};
            }
            Cookie.store();
        },
        getData: function(key) {
            return Cookie.data[key];
        },
        setData: function(key, value) {
            Cookie.data[key] = value;
            Cookie.store();
        },
        removeData: function(key) {
            delete Cookie.data[key];
            Cookie.store();
        },
        retrieve: function() {
            var start = document.cookie.indexOf(Cookie.options.name + "=");
    
            if(start == -1) {
                return null;
            }
            if(Cookie.options.name != document.cookie.substr(start, Cookie.options.name.length)) {
                return null;
            }
    
            var len = start + Cookie.options.name.length + 1;   
            var end = document.cookie.indexOf(';', len);
    
            if(end == -1) {
                end = document.cookie.length;
            } 
            return unescape(document.cookie.substring(len, end));
        },
        store: function() {
            var expires = '';
    
            if (Cookie.options.expires) {
                var today = new Date();
                expires = Cookie.options.expires * 86400000;
                expires = ';expires=' + new Date(today.getTime() + expires);
            }
    
            document.cookie = Cookie.options.name + '=' + escape(Object.toJSON(Cookie.data)) + Cookie.getOptions() + expires;
        },
        erase: function() {
            document.cookie = Cookie.options.name + '=' + Cookie.getOptions() + ';expires=Thu, 01-Jan-1970 00:00:01 GMT';
        },
        getOptions: function() {
            return (Cookie.options.path ? ';path=' + Cookie.options.path : '') + (Cookie.options.domain ? ';domain=' + Cookie.options.domain : '') + (Cookie.options.secure ? ';secure' : '');      
        }
    };
    

### Example Usage

Currently, the Cookie class only handles a single named cookie. This is acceptable since you can store multiple variables in a single cookie. However, I want to refactor this class from a Singleton to a Factory. Look for that in the future. In the meantime, here are some current sample uses:

Cookie that expires 90 days from visit, and sets a value:

    Cookie.init({name: 'yourdata', expires: 90});
    Cookie.setData('favorites', false);
    

Cookie that only lasts the session, with default data:

    Cookie.init({name: 'mydata'}, {foo: 'bar', x: 0});
    alert(Cookie.getData('foo'));
    

### A Few Sweet Spots

I wanted all the cookie variables to be stored in a single cookie. The class does allow you to still make independent Cookies. In order to store data, I would need some format to store my Cookie data. What better than JSON, and Prototype has a `toJSON` method.

I also wanted to encapsulate the cookie data. Plus since I was storing the cookie data as a Hash, I may have future changes. So the `getData`, `setData` accessor methods can be used for data management. And In *Rails-esk* fashion, I auto-save the cookie at the end of `setData`.

Finally, the Cookie auto-loads or is auto-created depending on the name passed to `init`. It will look in `document.cookie` and if it doesn't exist it will create a cookie with the settings provided. Finally, it loads the cookie data.

### In Closing

Prototype did not natively extend JavaScript with a Cookie object. However, by leveraging a few other classes, and the scripts mentioned above, I came up with a quick solution. Given, this class does not contain everything and could benefit from a code review. In the future, I may revisit the loading and possibly build some convenience methods to allow `Cookie['key']` instead of `Cookie.getData('key')`. Yet for what it does in 60 lines, it is more than able to handle my front-end needs.

 [1]: http://prototypejs.org
 [2]: http://scriptaculous.com
 [3]: http://www.dustindiaz.com/top-ten-javascript/
 [4]: http://ajaxian.com/archives/cookiejar-json-cookies
