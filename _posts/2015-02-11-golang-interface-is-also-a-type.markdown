---
layout: post
title: "Golang: interface is also a type"
date: 2015-02-11 21:39:52 +0800
comments: true
categories: [Golang]
---

An `interface` is two things: [it is a set of methods, but it is also a type](http://jordanorelli.com/post/32665860244/how-to-use-interfaces-in-go).

When you first learn Go, `interface` is always introduced as a common methods for some types.

A lesser known feature is that `interface{}` is a type - the empty interface with no methods. Therefore all types _have already implemented_ `interface{}`.

One good use is in `map[string]interface{}` - where the key is `string` and the value is _any type_. You can read more about [interface values](http://research.swtch.com/interfaces).
