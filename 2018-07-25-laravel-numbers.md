---
layout: post
title: "Laravel by the Numbers"
date: 2018-07-25 16:30
comments: true
categories:
  - Main Thread
description: 'A unique analysis of data from Laravel Shift on over 8,000 Laravel apps.'
subheading: 'A unique analysis of data from Laravel Shift on over 8,000 Laravel apps.'
excerpt: 'A unique analysis of data from Laravel Shift on over 8,000 Laravel apps.'
---
I had the privilege to speak at Laracon again this year. The last Laracon talk I gave, [Practicing YAGNI](https://streamacon.com/video/laracon-us-2016/jason-mccreary-yagni-with-laravel), is one I am most proud of. Initially, I wanted to do a continuation on this topic. But there were some other talks on related topics. So I thought, "what can I talk about that's unique to me".

The answer was [Laravel _Shift_](https://laravelshift.com). As the creator of _Shift_ I have a unique insight into Laravel apps.

I'm super sensitive about sounding _salesy_. I don't want to talk about _Shift_ itself. I want to talk about the data derived from _Shift_.

At the time of this writing, _Shift_ has upgraded over 8,500 Laravel apps. Every time a _Shift_ runs a log file is created. Initially, these log files were for debugging. A way for me to not only offer support, but log events that let me know how I might improve the services.

For full transparency, here's an example of one of the log files. Other than the _Shift_ number, the data is completely anonymous. No code copied. No API tokens. Just simple log messages.

```txt
*** _Shift_ version: 0281cff03fee62f250253f1e485dc7a790209866
*** Cloning app...

>>> _Shift_ 5.2 Event: app did not contain references to SelfHandling
>>> _Shift_ 5.2 Event: could not upgrade middleware Data: ["app\/Http\/Middleware\/Authenticate.php","app\/Http\/Middleware\/EncryptCookies.php","app\/Http\/Middleware\/RedirectIfAuthenticated.php"]
>>> _Shift_ 5.2 Event: could not upgrade app/Providers/RouteServiceProvider.php
>>> _Shift_ 5.2 Event: could not patch app/Providers/RouteServiceProvider.php
>>> _Shift_ 5.2 Event: could not find User model path
>>> _Shift_ 5.2 Event: found additional uses of Event names
>>> _Shift_ 5.2 Event: matched core file: phpunit.xml with version: 5.1.11
>>> _Shift_ 5.2 Event: matched core file: tests/TestCase.php with version: 5.1.33
>>> _Shift_ 5.2 Event: matched core file: database/migrations/2014_10_12_100000_create_password_resets_table.php with version: 5.1.33
>>> _Shift_ 5.2 Event: matched core file: public/.htaccess with version: 5.1.33
>>> _Shift_ 5.2 Event: matched core file: config/broadcasting.php with version: 5.1.11
>>> _Shift_ 5.2 Event: matched core file: config/cache.php with version: 5.1.33
>>> _Shift_ 5.2 Event: matched core file: config/compile.php with version: 5.1.33
>>> _Shift_ 5.2 Event: matched core file: config/database.php with version: 5.1.11
>>> _Shift_ 5.2 Event: matched core file: config/filesystems.php with version: 5.1.33
>>> _Shift_ 5.2 Event: matched core file: config/queue.php with version: 5.1.11
>>> _Shift_ 5.2 Event: matched core file: config/view.php with version: 5.1.33
>>> _Shift_ 5.2 Event: could not upgrade config files Data: {"1":"config\/auth.php","2":"config\/mail.php","3":"config\/services.php","4":"config\/session.php"}
>>> _Shift_ 5.2 Event: could not upgrade package.json
>>> _Shift_ 5.2 Event: app contained phpspec/phpspec requirement of ~2.1
>>> _Shift_ 5.2 Event: found customized namespace

*** _Shift_ ran in: 212.209509
```

Now this doesn't seem like much. But we can derive a lot of information from these simple messages. What files were upgraded. What features were used. What packages were used.

I mined these log files and want to share a dozen metrics and insights. I'm not a _data guy_. I've done my best to create some simple metrics and draw some conclusions from them in an effort to help developers craft conventional, upgradable Laravel apps.

## Most Popular Laravel Version

**Laravel 5.3 is the most popular version.** A lot of apps get stuck on Laravel 5.3 because of PHP version requirements, conversion to Dusk, and changes in auth components.

However, we have to remember, _Shift_ is an upgrade service. So while this indicates Laravel 5.3 is popular _in the wild_, it equally indicates apps are being upgrade. As such, Laravel 5.5 is likely the most popular version.

## Most Popular Packages
Here are the top 15 packages used in Laravel applications. _**Note:** This sample size is smaller and limited to apps running Laravel 5.5 or higher. Core packages included by Laravel are excluded._

- **58%** of apps use `guzzlehttp/guzzle`
- **36%** of apps use `predis/predis`
- **34%** of apps use `laravelcollective/html`
- **32%** of apps use `league/flysystem-aws-s3-v3`
- **27%** of apps use `intervention/image`
- **25%** of apps use `maatwebsite/excel`
- **24%** of apps use `spatie/laravel-backup`
- **23%** of apps use `laravel/horizon`
- **22%** of apps use `bugsnag/bugsnag-laravel`
- **21%** of apps use `laravel/socialite`
- **20%** of apps use `laravel/passport`
- **19%** of apps use `sentry/sentry-laravel`
- **15%** of apps use `spatie/laravel-permission`
- **14%** of apps use `laravel/scout`
- **14%** of apps use `league/csv`

Also, popular _development_ packages:

- **35%** of apps use `barryvdh/laravel-debugbar`
- **28%** of apps use `barryvdh/laravel-ide-helper`
- **19%** of apps use `laravel/dusk`
- **11%** of apps use `laravel/browser-kit-testing`

## Most Changed File
**Config files are the most changed files.** While this makes sense, it also makes an app less upgradable. Often there are other ways to make these changes and keep the config files defaulted.

**Be sure you're leveraging environment variables**. Many apps overwrite the default value instead of setting the `ENV` value. For example, consider the `config/mail.php`:

```php
'from' => [
    'address' => env('MAIL_FROM_ADDRESS', 'shift@laravelshift.com'),
    'name' => env('MAIL_FROM_NAME', 'Laravel _Shift_'),
],
```

Instead, set the environment variables and preserve the default values so the config file remains unchanged:

```php
'from' => [
    'address' => env('MAIL_FROM_ADDRESS', 'hello@example.com'),
    'name' => env('MAIL_FROM_NAME', 'Example'),
],
```

**Create your own config file.**. When there are a log of configuration options, consider creating a new config file. I often name these `config/system.php`, `config/settings.php`, or something domain specific like `config/shift.php`. This prevents lots of changes being made to various config files and organizes them to one file.

## <s>Custom</s> Vanity Namespaces
**Leave the default _App_ namespace.**. While only 9% of apps change the namespace, this should be zero. Taylor Otwell, Jeffrey Way, and others have recommend for a while now leaving it default. From an upgrade perspective, this is just one more difference you have to remember to change.

If you've customized you namespace, you can change it back by running:

```sh
artisan app:name App
```

_**Note:** This will not change the records in the database for polymorphic relationships. You will need to write a query to change these._


## App Structure
Here are the top, non-core folders (and files) found under the `app` folder.

- _36%_ of apps contain `app/Models`
- _29%_ of apps contain `app/Services`
- _29%_ of apps contain `app/Helpers`
- _25%_ of apps contain `app/Traits`
- _20%_ of apps contain `app/Rules`
- _18%_ of apps contain `app/Repositories`
- _17%_ of apps contain `app/helpers.php`

A little more detail. `app/Models` was a controversial change from Laravel 4 to Laravel 5. Seems 1 in 3 application still namespace _models_ under `app/Models`. On average, this folder exists when there are more double-digit models (11 on average). This is likely for organization as not to clutter the `app` folder with files.

`app/Services` and `app/Helpers` are the next most common folders. These are likely _catch-all_ folders which contain classes of varying responsibilities. Definitely an opportunity for better naming and organization.

The `app/helpers.php` file is a common addition to  Laravel app. But it doesn't have a designated home. It is often put in the `app` folder. However as an un-namespaced file it's not autoloaded with the other files within app. I find `bootstrap` folder to be a more accurate location for its purposes - load functions necessary for the app.

## Inserting Inheritance
**23% of apps inject inheritance.** Often apps will inject a layer into the inheritance hierarchy. Most commonly a `BaseController` or `BaseModel` class. While inheritance is a pillar of object oriented programming, it is not always the right solution.

Instead of forcing a design, I like _grok the framework_. Whether it's the language or framework, I try to adapt to my surroundings. In the case of Laravel, there's no need to inject a `BaseController` as the framework inherits from a `Controller` you may customize.

As for models or other classes, Laravel uses traits more than inheritance to decorate classes with additional functionality. Traits may be a better fit for your code as well as more closely align it with the framework.

## Facade Abuse
**57% of apps abuse Facades.**. _Facades_ are already a controversial topic within the Laravel community. A majority of apps abuse facades adding fuel to the fire. The biggest offense occur within _controllers_ and _middleware_ abusing the `Request` and `Auth` facades.

Consider the following code for a controller action:

```php
public function store()
{
    $data = Request::only('product_id', 'token');

    if (Auth::check()) {
        $data['email'] = Auth::user()->email;
    }

    // ...
}
```

It uses the `Request` facade to access request data and the `Auth` facade for the authenticated user. However, both controllers and middleware have a _request_ object injected. Instead of using two facades we can get everything through this object. This not only prevents facade abuse, but helps lower coupling which is good for design and testing.

```php
public function store(Request $request)
{
    $data = $request->only('product_id', 'token');

    if ($request->user()) {
        $data['email'] = $request->user()->email;
    }

    // ...
}
```

## Queries in Views
**24% of apps have queries in views.** This goes against the MVC paradigm. _Views_ should not be interacting directly with _Models_. I know Eloquent makes it so easy. But use your _Controllers_ to broker the data and help bring this down to zero.

## Env Envy
**42% of apps access environment variables directly.** Laravel 5.4 offers configuration caching for a performance boast. This can be achieved by running `artisan config:cache`. However, in order to take advantage of this feature, you can not use the `env` helper within your code, only within config files. Instead, you must access the variable through the `config` helper.

## Actually Cruddy Controllers
**77% of apps have non-cruddy controller actions.** Adam Wathan gave an excellent talk at Laracon 2017 called [Cruddy by Design](https://www.youtube.com/watch?v=MF0jFKvS4SI). Essentially, we want to limit our controllers to the [7 resourceful actions](https://laravel.com/docs/5.6/controllers#resource-controllers). Anything else is an opportunity to create additional controllers. Unfortunately, a majority of apps don't follow this advise.

## Controller Validation
**89% of apps do validation with controllers.** While the latest versions of Laravel make validation super easy, this code grows quickly. As such, it can make controller actions pretty long. As an alternative, consider leveraging a [Form Request Object](https://laravel.com/docs/5.6/validation#creating-form-requests).

## Blade Directives
**71% of apps aren't using available directives.** Laravel has many native blade directives. In fact, not all of them are in the documentation, much less the [Blade section](https://laravel.com/docs/5.6/blade). So unless you troll all the tips on Twitter, you'll likely miss out.

The least used, but most helpful directives are: `@auth`, `@guest`, `@json`, `@method`, and `@csrf`. Of course, you can use PHP inside of Blade templates or create your own directives. But as noted before, I like to _grok the framework_ as much as possible. So my goal is to only use Blade directives within my templates, no PHP tags.

## Stats
I'll close with some additional stats and a few observations on them. _**Note:** This sample size is smaller and limited to apps running Laravel 5.5 or higher. Stats were generated using [laravel-stats](https://github.com/stefanzweifel/laravel-stats)._

```txt
+-------------------+-------+---------+---------------+------------+
| Name              | Usage | Classes | Methods/Class | LoC/Method |
+-------------------+-------+---------+---------------+------------+
| Commands          |   66% |    6.37 |          2.42 |       9.68 |
| Controllers       |   67% |   20.91 |          4.10 |       6.28 |
| Events            |   29% |    5.63 |          1.64 |       7.69 |
| Jobs              |   31% |    4.11 |          2.36 |      11.84 |
| Listeners         |   39% |    5.35 |          1.89 |       5.16 |
| Mails             |   39% |    6.72 |          2.06 |       6.71 |
| Middleware        |  100% |    4.22 |          0.12 |       4.93 |
| Models            |   99% |   17.80 |          3.75 |       3.79 |
| Notifications     |   39% |    4.57 |          3.80 |       3.71 |
| Policies          |   19% |    5.09 |          3.96 |       2.88 |
| Requests          |   58% |   11.04 |          2.07 |       2.52 |
| Resources         |    7% |   14.75 |          0.94 |       4.95 |
| Rules             |   17% |    2.40 |          3.07 |       2.86 |
| Service Providers |  100% |    6.28 |          1.97 |       4.61 |
| PHPUnit Tests     |  100% |    9.96 |          2.02 |       6.63 |
| Dusk Tests        |   18% |       5 |          3.00 |       6.67 |
| Browserkit Tests  |    3% |      13 |          4.16 |       6.05 |
+-------------------+-------+---------+---------------+------------+
| Total             |       |  144.54 |          2.18 |       5.72 |
+-------------------+-------+---------+---------------+------------+
```

- _Jobs_, _Controllers_, and _Events_ contain the highest lines of code per method stats, respectively.
- Although _PHPUnit Tests_ were listed at 100% usage, this is misleading. All Laravel projects come with _example tests_. Other metrics show that **only 27% of apps have custom tests**.
- There seems to be a bug with this package and counting _Controllers_ as likely more than _67%_ of apps use controllers.

---

_I hope these metrics and insights provide ways to improve your Laravel apps as well as promote maintainability. For a limited time, you can run the [Laravel Analyzer](https://laravelshift.com/opinionated-laravel-way-shift) for **free**. I plan to release more observations as the sample size grows._
