---
layout: post
title: "Writing Clean Code"
date: 2017-08-14 1:03
comments: true
categories:
  - Main Thread
description: 'Three simple practices to help write clean code and improve the readability of a codebase.'
subheading: 'Three simple practices to help write clean code and improve the readability of a codebase.'
excerpt: 'Three simple practices to help write clean code and improve the readability of a codebase.'
---
I recently started a new job. With every new job comes a new codebase. This is probably my twentieth job. So I've seen a lot of codebases.

Unfortunately they all suffer from the same fundamental issue - _inconsistency_. Likely the result of years of code patching, large teams, changing hands, or all of the above.

This creates a problem because **we read code far more than we write code**. As I read a new codebase these inconsistencies distract me from the true code. My focus shifts to the mundane of indentation and variable tracking instead of the important business logic.

Over the years, I find I [boy scout](https://jason.pureconcepts.net/2015/01/are-you-a-boy-scout/) a new codebase in the same way. I apply three simple practices to clean up the code and improve its readability.

To demonstrate, I’ll apply these to the following, real-world code I read just the other day.

```php
function check($scp, $uid){
  if (Auth::user()->hasRole('admin')){
    return true;
  }
  else {
  switch ($scp) {
    case 'public':
      return true;
      break;
    case 'private':
      if (Auth::user()->id === $uid)
        return true;
      break;
    default: return false;
  }
  return false;
  }
}
```

## Adopt a code style
I know I’m the 1,647th person to say, _“format your code”_. But it apparently still needs to be said. Nearly all of the codebases I’ve worked on have failed to adopt a code style. With the availability of powerful IDEs, pre-commit hooks, and CI pipelines it requires virtually no effort to format a codebase consistently.

If the goal is to improve code readability, then adopting a code style is the single, best way to do so. In the end, it doesn’t matter which code style you adopt. Only that you apply it consistently. Once you or your team agrees upon a code style, configure your IDE or find a tool to format the code automatically.

Since our code is PHP, I chosen to adopt the [PSR-2 code style](http://www.php-fig.org/psr/psr-2/). I used *PHP Code Beautifier* within [PHPCodeSniffer](https://github.com/squizlabs/PHP_CodeSniffer) to automatically fix the code format.

Here's the same code after adopting a code style. The indentation allows us to see the structure of the code more easily.

```php
function check($scp, $uid)
{
    if (Auth::user()->hasRole('admin')) {
        return true;
    } else {
        switch ($scp) {
            case 'public':
                return true;
            break;
            case 'private':
                if (Auth::user()->id === $uid) {
                    return true;
                }
                break;
            default:
                return false;
        }
        return false;
    }
}
```


## Naming things ~~properly~~ clearly
Yes, something else you’ve heard plenty. I know [naming things is hard](https://martinfowler.com/bliki/TwoHardThings.html). One of the reasons it’s hard is there are no clear rules about naming things. It’s all about context. And context changes frequently in code.

Use these contexts to draw out a name. Once you find a clear name, apply it to all contexts to link them together. This will create consistency and make it easier to follow a variable through the codebase.

Don't worry about strictly using traditional naming conventions. I often find codebases mix and match. A clear name is far more important than `snake_case` vs `camelCase`. Just apply it consistently within the current context.

If you’re stuck, use a temporary name and keep coding. I’ll often name things `$bob` or `$whatever` to avoid getting on stuck on a _hard thing_. Once I finish coding the rest, I go back and rename the variable. By then I have more context and have often found a clear name.

Clear names will help future readers understand this code more quickly. They don’t have to be *perfect*. The goal is to boost the signal for future readers. Maybe they can incrementally improve the naming with their afforded mental capacity.

After analyzing this code, I have more context to choose clearer names. Applying clear names not only improves readability, but boosts the context making the intent of the code easier to see.

```php
function canView($scope, $owner_id)
{
    if (Auth::user()->hasRole('admin')) {
        return true;
    } else {
        switch ($scope) {
            case 'public':
                return true;
            break;
            case 'private':
                if (Auth::user()->id === $owner_id) {
                    return true;
                }
                break;
            default:
                return false;
        }
        return false;
    }
}
```

## Avoid Nested Code
There are some hard rules regarding nested code. Many developers believe you should only allow one nesting level. In general, I tend to ignore rules with *hard* numbers. They feel so arbitrary given code is so fluid.

It's more that nested code is often unnecessary. I have seen the entire body of a function wrapped in an `if`. I have seen several layers of nesting. I have literally seen empty `else` blocks. Often adding guard clauses, inverting conditional logic, or leveraging `return` can remove the *need* to nest code.

In this case, I'll leverage the existing `return` statements and flip the `switch` to remove most of the nesting from the code.

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


In the end, coding is writing. As an author you have a responsibility to your readers. Maintaining a consistent style, vocabulary, and flow is the easiest way to ensure readability. Remove or change these and maintain readability you will not.

**Want to see these practices in action?** I'm hosting a free, one-hour workshop where I'll demo each of these practice, and more, through live coding. [Sign up](https://workshopsbyjmac.com) to secure your spot.
