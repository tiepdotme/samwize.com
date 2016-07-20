---
layout: post
title: "Mocking in Swift"
date: 2016-07-11T14:18:55+08:00
categories: [Swift]
---

https://realm.io/news/tryswift-veronica-ray-real-world-mocking-swift/

`OCMock` has severe limitations in Swift
because Swift does not have reflection

If you are not using a DI framework
you can still manually inject dependencies

DI - simply means giving an object it's instance variables

Constructor Injection
Have a default constructor with real ivar
have `convenience` constructor with DI injected objects

Stubbs - fake a method call

Partial Mocks
anti-pattern
_what is real? what is fake?_ 
It decreases comprehensibility of tests

Mocking with protocols
better than subclassing

What makes a good mock?

- Quick and easy to write
- Relatively short and does not contain much information you don’t need
- Legitimate reason to not use real object

More value type
less mocking needed

https://www.destroyallsoftware.com/talks/boundaries
https://realm.io/news/andy-matuschak-controlling-complexity/


## Partial mock an anti-pattern?

I disagree on this point [too](http://cleanswifter.com/swift-partial-mocks-are-not-an-bad/)

I get wary every time I hear a pattern is anti-pattern
because a pattern is only anti because of the way you use it WRONGLY

Partial mock is usually subclassing the actual object
and override part of it so as to "mock" it

It is a simple way to achieve mocking

With Swift nested type
testing with partial mocks defined right in the test case
makes clear _what is real, what is fake_

