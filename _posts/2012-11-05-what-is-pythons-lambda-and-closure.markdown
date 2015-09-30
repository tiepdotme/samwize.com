---
layout: post
title: "What is Python's Lambda and Closure"
date: 2012-11-05 20:06
comments: true
categories: Python
---

Lambda is

> one line anonymous function constructed on the fly

Closure is

> a function object that remembers values in enclosing scopes regardless of whether those scopes are still present in memory

<!-- more -->

## What is Lambda? ##

[DiveIntoPython](http://www.diveintopython.net/power_of_introspection/lambda_functions.html) gave a good definition. Lambda is 

- a one line function
- anonymous function (no name) 
- a function that can be used anywhere a function is required
- it takes in arbitrary parameters
- and implicitly return as a single expression

But most important, you need not use lambda.

Lambda is a **style**. You need not use it. 

What's more, if you need multiple lines, then consider a normal function. Python can pass function as parameter anyway.



## Why do we have Lambda? ##

You might [wonder](http://stackoverflow.com/questions/890128/python-lambda-why) why is there lambda in the first place.

This post on [Python history](http://python-history.blogspot.sg/2009/04/origins-of-pythons-functional-features.html) explained well.

> In 1994, lambda operator was introduced for creating anonymous functions (as expressions). Lack of a better choice..

However, the choice of the terminology "lambda" had many unintended consequences. It doesn't match the expectation from other languages. Hence it is usually considered "sorely lacking" in features. eg. It does not work with surrounding codes. 

Lambda is no closure like other languages.



## What is Closure? ##


Best explained in [ShutUpAndShip](http://www.shutupandship.com/2012/01/python-closures-explained.html), 

> A CLOSURE is a function object that remembers values in enclosing scopes regardless of whether those scopes are still present in memory.

In another shorter [explanation](http://ynniv.com/blog/2007/08/closures-in-python.html),

> A closure is data attached to code

In Python,

- Methods are closures
- Functions are NOT closures
- Lambda is NOT closure


## Huh? Method vs Function ##

In case you wonder what is the difference between [Method & Function](http://stackoverflow.com/a/155633/242682),

> A method is on an object. A function is independent of an object.

You define a **function** such as `def foo():`. 

You use a **method** of an object such as `obj.foo()`. `obj` is implicitly passed as the first parameter to the method `foo`. That's why in a class, you have a `self` as the first param of a _method_.
