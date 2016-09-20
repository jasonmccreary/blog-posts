---
layout: post
title: "The Proximity Rule?"
date: 2016-08-19 19:37
comments: true
categories:
  - Main Thread
description: 'A recent coding style I call the "Proximity Rule". Might be called something else, but I still like the code.'
subheading: 'A recent coding style I call the "Proximity Rule". Might be called something else, but I still like the code.'
excerpt: 'A recent coding style I call the "Proximity Rule". Might be called something else, but I still like the code.'
---
I noticed a recurring style in my code lately. Actually, my pair noticed and asked me about it. I call it the "Proximity Rule". I wanted to call it the "Proximity Principle", but the initialism made my inner-child chuckle.

I feel it's easier to explain with code samples. Consider the following function which filters items in an array using a callback.

```javascript
function array_filter(array, callback) {
    var i;
    var length = array.length;
    var filtered = [];

    for (i = 0; i < length; ++i) {
        if (callback(array[i])) {
            filtered.push(array[i]);
        }
    }

    return filtered;
}
```

This code has a simple, traditional style. It groups similar statements together into blocks of code separated by whitespace. Each group tells a story - initialize, execute, respond.

However, this story is a bit robotic. Fine for the computer, but humans need to read this story too. Let's look at the same code after applying the *Proximity Rule*.

```javascript
function array_filter(array, callback) {
    var i;
    var filtered = [];

    var length = array.length;
    for (i = 0; i < length; ++i) {
        if (callback(array[i])) {
            filtered.push(array[i]);
        }
    }

    return filtered;
}
```

By moving the `length` assignment closer to the `for` loop I emphasize their relationship. So the *Proximity Rule* is not just about grouping similar statements of code. Its also about grouping *related code*.

Let's look at another example. Consider the following tests for our `array_filter` function.

```javascript
describe("array_filter", function() {
    var actual;
    var odd_callback;
    var even_callback;

    beforeEach(function() {
        odd_callback = jasmine.createSpy('odd');
        odd_callback.and.returnValues(true, false, true, false, true);

        even_callback = jasmine.createSpy('even');
        even_callback.and.returnValues(false, true, false, true, false);
    });

    describe("when filtering odd numbers", function() {
        beforeEach(function() {
            actual = array_filter([1, 2, 3, 4, 5], odd_callback);
        });

        it("should return only odd numbers", function() {
            expect(actual).toEqual([1, 3, 5]);
        });
    });

    describe("when filtering even numbers", function() {
        beforeEach(function() {
            actual = array_filter([1, 2, 3, 4, 5], even_callback);
        });

        it("should return only even numbers", function() {
            expect(actual).toEqual([2, 4]);
        });
    });
});
```

We again see code grouped by statement. If we focus on the context *when filtering even numbers*, we might ask ourselves, "What is `even_callback`?"

If we apply the *Proximity Rule*, we can improve the readability and eliminate this question.

```javascript
describe("array_filter", function() {
    var actual;

    describe("when filtering odd numbers", function() {
        beforeEach(function() {
            var callback = jasmine.createSpy('odd');
            callback.and.returnValues(true, false, true, false, true);
            actual = array_filter([1, 2, 3, 4, 5], callback);
        });

        it("should return only odd numbers", function() {
            expect(actual).toEqual([1, 3, 5]);
        });
    });

    describe("when filtering even numbers", function() {
        beforeEach(function() {
            var callback = jasmine.createSpy('even');
            callback.and.returnValues(false, true, false, true, false);
            actual = array_filter([1, 2, 3, 4, 5], callback);
        });

        it("should return only even numbers", function() {
            expect(actual).toEqual([2, 4]);
        });
    });
});
```

This example also demonstrates how the *Proximity Rule* can help condense code. Especially when [Code Smells](https://blog.codinghorror.com/code-smells/), such as *Lazy Class*, are in the air.

For me, the *Proximity Rule* is simply a coding style. One I by no means think this is new. I am sure Knuth or Beck or one of the other Programming Godfathers have written about this. If so, please let me know. One of my recent goals is to call things by their proper name. To be fair, I did attempt to [ask on Twitter](https://twitter.com/gonedark/status/753278729556664320).
