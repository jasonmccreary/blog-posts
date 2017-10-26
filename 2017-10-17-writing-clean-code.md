---
layout: post
title: "Writing Clean Code (Part 2)"
date: 2017-10-17 1:03
comments: true
categories:
  - Main Thread
description: 'Three more practices to write clean code and help improve code readability.'
subheading: 'Three more practices to write clean code and help improve code readability.'
excerpt: 'Three more practices to write clean code and help improve code readability.'
---
In [Part 1 of Writing Clean Code](https://dev.to/gonedark/writing-clean-code) I outlined three simple practices of formatting, naming, and avoiding nested code. All in an effort to improve code readability.

In Part 2, I want to go a little deeper and cover _grouping_. When I say grouping, I'm really talking about the Object Oriented Programming paradigm of _encapsulation_. Whether we group the code into a function or a class is often not important. What is important is did we improve the readability of the code.

To measure our change, we should ask:

> Did we improve readability?

Admittedly a bit subjective, but you push yourself to stay objective. I've been pair programming for the last two years. Developers tend to agree on fundamental readability. Where we differ at the edges. These nuances can lead to some pretty great discussion.

_What to group_ is often easy to identify. We can all point out the code we don't like. We said _how to group_ the code is not important. The question that remains is _when to group code_. When do I clean up the code by grouping?

Let's look at three _motivations_ for grouping code.

## Improving communication

Any bit of code which requires additional context is ripe for grouping. I prefer when my code is not written in a way that requires me to know the business logic. No matter how simple the implementation, I'll never understand. By grouping this code, we provide an additional layer of abstraction. A way to shield ourselves and future developers from the inherit complexity of the system.

Consider our previous code sample:

```php
function canView($scope, $owner_id)
{
    if ($scope === 'public') {
        return true;
    }

    if (Auth::user()->hasRole('admin')) {
        return true;
    }

    if ($scope === 'private' && Auth::user()->id === $owner_id) {
        return true;
    }

    return false;
}
```

While the logic is straightforward, we improve communication by extracting it into contextually named helper methods.

```php
function canView($scope, $owner_id)
{
    if ($scope === 'public') {
        return true;
    }

    if (isAdmin()) {
        return true;
    }

    if ($scope === 'private' && isOwner()) {
        return true;
    }

    return false;
}
```

Methods like `isAdmin()` and `isOwner` relay business logic making it easy to understand. Once understood I can apply it easily to other areas of the codebase. In the end, we didn't just improve communication, we also _taught_ the developer about the code.

It's important to point out that I didn't group all of the code. There is a mental cost for every grouping. Each needs to provide enough value to cover its cost. In this case, I didn't group logic into a `hasScope()` function as it not only didn't improve communication, but the method signature is just as verbose as the expression.

## Couple data
Another principle in programming is _low coupling_. Coupling is not bad. In fact, it's good when data or code truly belongs together. We can identify areas for coupling by spotting logical connections or by a similar _rate of change_.

Consider the following code sample:

```php
function plot($x, $y, $z)
{
    // ...
}

function transfer($amount, $currency)
{
    // ...
}

function substring($string, $start, $length)
{
    // ...
}
```

Only the function signatures here as I want to focus on the parameters. You may not see the grouping opportunity right away. No worries, it's because we all suffer from [primitive obsession](https://refactoring.guru/smells/primitive-obsession). Nothing wrong with using primitives. But our propensity to _only_ use them may prevent us from grouping this data into an object.

```php
function plot(Point $point)
{
    // ...
}

function transfer(Money $money)
{
    // ...
}

function substring($string, Range $range)
{
    // ...
}
```

By encapsulating the data within an object, we not only improve the coupling but also provide a place for additional logic. We can move any inline logic related to this data to the object. Take a minute to read Martin Fowler's [Range object](https://martinfowler.com/eaaDev/Range.html) for a more in-depth example.

## Organizing code
Last but not least is simply organization. Don't be afraid to split similar code into its own function or class. So long as it carries its own weight, it will likely improve the codebase.

Spotting these is more high level than spotting _data coupling_. Here we take more of a visual approach. If something doesn't seem to match the local aesthetics, it may belong elsewhere.

Consider the following code sample:

```php
namespace App\Models;

class User
{
    public function find($id) {}

    public function create() {}

    public function save() {}

    public function destroy() {}

    public function displayName() {}

    public function displaySignature() {}

    public function displaySalutation() {}

    public function createBadge() {}

    public function printBadge() {}
}
```

Here we have a model. Typically a model primarily contains the CRUD methods (create, read, update, delete). While it's perfectly acceptable for the model to contain additional methods, we may notice relationships between these other methods in the model.

By name alone we can spot methods related to _display_ and other _badge_ methods. We may be able to organize this code elsewhere. In this case, the _display_ methods can be extracted to a _Presenter_ class. The _badge_ methods to a _Printer_ class or into its own `Badge` object.


Give these motivations a try. Maybe some work for your codebase, maybe some don't. The answer to _did we improve code readability_ may vary from developer to developer and project to project. But always ask the question…
