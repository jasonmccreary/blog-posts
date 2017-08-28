---
layout: post
title: "Accepting Adam's TDD Challenge"
date: 2017-02-27 11:37
comments: true
categories:
  - Main Thread
description: "A response to Adam Wathan's challenge that isolated unit testing is incompatible with TDD."
subheading: "A response to Adam Wathan's challenge that isolated unit testing is incompatible with TDD."
excerpt: "A response to Adam Wathan's challenge that isolated unit testing is incompatible with TDD."
---
Last week at [Laracon Online](https://laracon.net) Adam Wathan gave a talk entitled *"Lies you've been told about testing"*. Following the talk, Adam posted [a challenge](https://twitter.com/adamwathan/status/839930049453244416). Amid Adam's post, he makes a single claim and presents a challenge.

### The claim
> isolated unit testing is incompatible with TDD

### The challenge
> Write a unit test that tests in isolation from its collaborators and passes for all three implementations.

As a member of an extreme programming team, I have practiced TDD every day for the past 2 years. As such, I'm compelled to accept the challenge. However, I'm going to focus first on *the claim*.

The premise of Adam's claim is centered around the *refactor* phase of TDD. Yet, there are other phases of TDD which can make the challenge easier.

TDD is about driving your implementation through tests. So, if we're talking about TDD, it doesn't make sense to go from implementations to your test.

Nonetheless, I want to accept the spirit of the challenge. So, let's follow the **full TDD process** and see where we end up.

## The missing TDD phases
While *refactor* is the final phase in the TDD process, there are two others.

1. The *red* phase where we write a failing test
2. The *green* phase where we make the test pass

However, a core tenant of the TDD process is that we only write enough code to make the test pass. This promotes doing the simplest thing possible.

In this case, I would go through several *red*/*green* cycles testing:

1. Class existence
2. Method existence
3. Method returns value
4. Injection of `Redirector`
5. Method returns response from `Redirector`
6. Injection of `CommandBus`
7. `CommandBus` is called
8. `CommandBus` is called with `Command` built with `Request` data

The resulting implementation might look something like:

```php
<?php

class ProductsController extends Controller
{
    private $commandBus;
    private $redirector;

    public function __construct(CommandBus $commandBus, Redirector $redirector)
    {
        $this->commandBus = $commandBus;
        $this->redirector = $redirector;
    }

    public function store(Request $request)
    {
        $command = new AddProductCommand(
            $request->user()->id(),
            $request->name,
            $request->description,
            $request->price
        );

        $this->commandBus->dispatch($command);

        return $this->redirector->to('/products');
    }
}
```

## Testing Styles
The background for this challenge comes from Adam's frustrations regarding *testing styles* ("Classist" vs "Mockist"). It's important to point out that TDD does not dictate a *testing style*. The only *style*, if any, is *minimal amount of code to make the test pass*.

In this case, since `CommandBus` and `Redirector` are under contract, *mocking* likely requires less test code. We could simply mock the interface and reliably stub and verify collaboration.

While we could also mock the `Request` object, it's used in several places and primarily represents a data object. As such, mocking would require more test code than testing with a *real* `Request` object. So, we'll just use a real one.

## Refactoring
So, we've reached the final TDD phase - *refactoring*. Let's see how we're doing.

Much of the variance between Adam's implementations was avoided. For example, by following the full TDD process we would not have required an `Auth` dependency. Anything we need we can get from the `Request` object.

We would also be able to freely refactor our use of the `Request` object since we are testing with a *real* one.

That leaves one bit of variance to support Adam's claim - refactoring the use of the `Redirector`.

There are a few options:

- The change to a named route could be considered a *new feature* as it requires a test to drive the existence of the named route.
- An agreement on a code standard. For example, always use named routes.
- Abstracting the `Redirector` interface. In this example, `Redirector` is part of the Laravel framework. As such, it's not something we *own* potentially making it harder to test.

It seems this last option is what Adam is tired of hearing. And so, I concede that some coupling between the test and implementation does exist. As such, there will be times a refactor requires a change in the corresponding test.

However, the *refactor* phase *should* include **refactoring the test**. If there is a better, simpler, or more consistent way to do something by all means change it.

In the end, I believe Adam's frustration is not with TDD or unit tests, but the specificity of the test code. I too have never liked when a test matches an implementation line for line. But this should indicate an opportunity to improve either the test or implementation.

Stepping back from the code, I think there are two final take aways:

- **Don't throw the baby out with the bath water**. Any practice has limitations. Part of mastering a practice is knowing how to work through them. If the refactor phase is hard - determine why it's hard, don't claim TDD is broken.
- **There are no silver bullets**. You'll receive a lot of *advice*. Especially in programming. Remember to keep an open mind. Especially when you're learning. Don't get caught up in the *testing styles* or refactor phase. Just keep doing the simplest thing that works.
