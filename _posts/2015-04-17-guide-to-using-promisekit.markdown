---
layout: post
title: "Guide to using PromiseKit"
date: 2015-04-17 08:49:54 +0800
comments: true
categories: [iOS]
---

[PromiseKit](http://promisekit.org) is a wonderful library for dealing for async callbacks. 

Other similar libraries are [Bolts Framework](http://promisekit.org) and [ReactiveCocoa](https://github.com/ReactiveCocoa/ReactiveCocoa), but personally I felt the syntax and design of PromiseKit to be much better.

Initially, I wanted to write a guide on some common usage of PromiseKit, but I find that the [common recipes by Phil Mitchell himself](http://philmitchell.github.io/PromiseKit/) is great.

Therefore, I will be emphasizing only on some usage/pitfall.

<!-- more -->

## Throwing NSError

You can return `NSError` (or `@throw` a message), which will then have the nearest `catch` blcok to handle


## Return in a promise

In a promise, you can return almost anything.

- If return a promise, next `then` will receive the _fulfilled value_
- If return `NSError`, next `catch` will handle it
- If return any other type, next `then` will recive it
- If return `nil`, that's fine too
- If no return, next `then` will run immediately

It's really flexible with promises.

Pitfall: If returning **different types**, the return type must be explicitly `(^id)` (any type), not just `(^)` (no type).

```objc
myURLPromise.then(^id(NSURL *url){ 
     if (url) {
         return url; // Got my url, I'm done, return the value
     }
     else {
         return myOtherURLPromise; // Not done, do some more work
     }
}).then(^(NSURL *url){
    // Do something with URL ...
});
```

