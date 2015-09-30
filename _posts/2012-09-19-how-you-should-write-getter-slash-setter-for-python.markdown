---
layout: post
title: "How you should write getter/setter for Python"
date: 2012-09-19 18:49
comments: true
categories: Python
published: true
---

Coming from the Java world, I am used to writing getter/setter for every member variables that I want to expose. (truth is I made use of IDE to auto generate)

I hate how much code Java has.

With it's philosophy of readable code, Python is different. 

<!-- more -->

### Getter ###

To write a getter for this member `foo`:

``` python
class MyClass(object):
	
	def __init__(self):
		self._foo = 1

	@property
	def foo(self):
		return self._foo
```

It uses an annotation `@property`. Then to access the property:

``` python
my_class = MyClass()
print my_class.foo
# 1
```

### Setter ###

To set a memter, use the annotation `@foo.setter` (replace foo with your member name).

``` python
class MyClass(object):
	...

	@foo.setter
	def foo(self, value):
		self._foo = value
```

<!-- http://stackoverflow.com/questions/6618002/python-property-versus-getters-and-setters -->