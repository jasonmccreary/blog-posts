---
layout: post
title: SMS admin in 8 lines of code
date: 2017-09-20 11:03
comments: true
categories:
  - Main Thread
description: 'In an effort to provide on the go support for Laravel Shift, I created an SMS based admin using Nexmo and Laravel.'
subheading: 'In an effort to provide on the go support for Laravel Shift, I created an SMS based admin using Nexmo and Laravel.'
excerpt: 'In an effort to provide on the go support for Laravel Shift, I created an SMS based admin using Nexmo and Laravel.'
---
I'm passionate about the products I build. But I try to balance my time between them. This can be difficult with a SaaS product. Especially a paid SaaS product like [Laravel Shift](https://laravelshift.com). You don't want to miss out on revenue.

While Shift is a fully automated service, there are times where human intervention is required. For example, if a customer uses an alternative form of payment or a Git related issue occurs. In which case I have to run the Shift manually. This involves:

1. Connecting to the server
2. Looking up the Shift
3. Creating a job
4. Pushing the job to the queue

I pride myself on support. Unless I'm sleeping, I try to handle every issue within an hour. Even though the steps above take under a minute, I would rather spend that time on something else. Your products should be fun, not a burden.

I know you developers are thinking - why don't you build a web administration? Well, [YAGNI](https://en.wikipedia.org/wiki/You_aren%27t_gonna_need_it) - that's why.

Consider the cost to develop (and maintain) such a web admin. I need to build login, listing/detail pages, search, and the action itself. Also from an experience side, it's roughly the same number of steps as before. Sure, it's easy to build with any framework. But I don't _need_ it.

In the end, all I _need_ is a **quick way** to run a Shift **on the go**. Looking back on almost two years of support, I often have the Shift number readily available. Creating the job and adding it to the queue is at most two lines of code. So the steps are not the pain point.

The pain point is *connecting to the server*. Unless I want to carry my laptop around, I can't connect to the server to run the Shift. (I actually have taken my laptop with me during peak times.)

What do I carry around with me all the time? My phone. I'm already reviewing the support emails from my phone. Wouldn't it be great when I need to run a Shift manually to just reply or _send a text_.

## Show me the code
Although the backstory relays the importance of finding the _right_ solution, I know you developers want to see the code.

As the SMS platform I used [Nexmo](https://www.nexmo.com/). Services like Nexmo make handling SMS communication pretty easy. I rent a phone number and set a webhook. That's it.

Now anytime that phone number receives a text, Nexmo sends data to this web endpoint. The data contains meta information like a unique message ID and timestamps, as well as the sending phone number and message body.

Really all I have to develop is a web endpoint. Shift is built with [Laravel](https;//laravelshift.com). This means I can quickly generate an API resource by running `php artisan make:controller --resource Api/SmsController`.

I remove the _extra_ actions as I only want the `store` endpoint. Since Nexmo sends a `POST` request this was a natural choice. It also fits from a _RESTful_ perspective as this endpoint _creates_ a new job and places it on the queue.

```php
class SmsController extends Controller
{
    public function store(RerunRequest $request)
    {
        $order = Order::findOrFail($request->get('text'));

        Queue::push(new PerformShift($order));

        return null;
    }
}
```

Here's the corresponding route:

```php
Route::apiResource('sms', 'Api\SmsController', ['only' => ['store']]);
```

Those familiar with Laravel may have noticed `RerunRequest` passed to `store()`. This is a [Form Request](https://laravel.com/docs/5.5/validation#creating-form-requests) which handles request validation in Laravel. In this case, it performs some very basic checks to ensure the message is as expected.

```php
class RerunRequest extends FormRequest
{
    /**
     * Determine if the user is authorized to make this request.
     *
     * @return bool
     */
    public function authorize()
    {
        return true;
    }

    /**
     * Get the validation rules that apply to the request.
     *
     * @return array
     */
    public function rules()
    {
        return [
            'msisdn' => 'required|integer|in:15025555555',
            'text' => 'required|integer',
        ];
    }
}
```

As an aside, I was surprised given all the [available validation rules in Laravel](https://laravel.com/docs/5.5/validation#available-validation-rules) there wasn't an `equals` or `is` rule. I had to use `in` to validate the sending phone number was my phone number.

That's it. A few generated classes and literally 8 lines of custom code. It fulfills all my requirements. I can maintain my speedy level of support from anywhere I have a cell signal. Even better, it saves me time. What used to take a minute now takes seconds. I've already used it several times.


## Why Nexmo?
It's _FREE_ to receive a text. This means all I pay for is a phone number. That costs less than $10/year - which is roughly the cost of one Shift. So, while there are other platforms, Nexmo literally fit the bill.

**Have something you want to build?** Let's pair up! I offer [pair programming sessions](https://coaching.pureconcepts.net) where we can tackle your projects together. It's a great way to crank out some code and level up our skills.
