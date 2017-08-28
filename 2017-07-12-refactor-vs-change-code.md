---
layout: post
title: "You changed the code, you didn't refactor the code."
date: 2017-07-05 10:37
comments: true
categories:
  - Main Thread
description: 'A closer look at the important difference between changing code and refactoring code.'
subheading: 'A closer look at the important difference between changing code and refactoring code.'
excerpt: 'A closer look at the important difference between changing code and refactoring code.'
---
There was a [good discussion on Twitter](https://twitter.com/JacobBennett/status/884776645743267845) yesterday regarding a code contribution to the [Laravel framework](https://github.com/laravel/framework). It ended with some good questions about the distinctions between “refactoring” vs “changing” code.

While I want to focus on these distinctions, let’s first focus on the code change.

Here’s the original code:


```php
    public static function before($subject, $search)
    {
        if ($search == '') {
            return $subject;
        }


        $pos = strpos($subject, $search);

        if ($pos === false) {
            return $subject;
        }

        return substr($subject, 0, $pos);
    }
```

And the “refactored” code:


```php
    public static function before($subject, $search)
    {
        return empty($search) ? $subject : explode($search, $subject)[0];
    }
```


A nice, clean one liner. All tests passed and the code was merged.

Developers with a keen testing eye may have already noticed an issue, but most noticed I quoted “refactored”.

That’s because this code wasn’t “refactored” it was “changed”. Let’s recall the definition of “refactor”.

> to restructure software without changing its observable behavior

In this case, because the new code behaves differently than the original, the observable behavior changed.

How does it behave differently?

This takes that keen testing eye, but a ready example is when `$search` is `0`. The original code would search within `$subject` and return the string _before_ the `0` occurrence. Whereas the new code would return early with `$subject`. Not the same behavior.

Unfortunately the existing tests did not catch this. As such, it was on the contributor to spot this _changed behavior_ - which they later did and [submitted a patch](https://github.com/laravel/framework/pull/19995) with the missing test. Upon doing so, this became a true refactor and great work!

However, this lead to another interesting question - since all the existing tests passed, was the original contribution a successful refactor.

Given the symbiotic relationship between refactoring and testing, some consider the tests to be the requirements. So if all tests pass, you met the requirements.

I think that’s a slippery slope. For me, the definition of “refactoring” again provides the answer through its own question - did we change the _observable_ behavior?

In this case, yes - we can observe the behavior changed. So the original contribution was not a true refactor, despite the passing tests.

Nonetheless, I think there are some other interesting points around refactoring and testing. Ones I will explore in a future post. For now, be mindful you’re truly “refactoring” code and not “changing” code.
