---
layout: post
title: "How to write Getter/Setter for static variables"
date: 2012-09-20 14:14
comments: true
categories: Python
---

In the last post, I blogged about how you should write [Getter/Setter for member variables](/2012/09/19/how-you-should-write-getter-slash-setter-for-python/). 

This is a follow-up for **static variables**, instead of instance variables.

<!-- more -->

I didn't know the answer to that, until I searched around [Stackoverflow](http://stackoverflow.com/questions/128573/using-property-on-classmethods). There are a couple of ways around using @property on classmethods.

The best answer for me is this:

``` python
class MyClass(object):
    _foo = 5
    class __metaclass__(type):
        @property
        def foo(cls):
                return cls._foo
        
        @foo.setter
        def foo(cls, value):
                cls._foo = value
```

It uses `__metaclass__`, some kind of [black magic](http://www.voidspace.org.uk/python/articles/five-minutes.shtml) in Python.

With that, you can use the getter/setter on the static variable.

``` python
>>> MyClass.foo
5
>>> MyClass.foo = 3
>>> MyClass.foo
3
```