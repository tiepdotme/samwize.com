---
layout: post
title: "Primer Guide to Combine Framework"
date: 2019-06-12T17:49:11+08:00
categories: [Foundation]
published: false
---

Combine framework is basically the React/RxSwift, but now build by Apple, as a first party framework.

Being first party, they have the power to improve the Swift language and compiler, and the whole syntax becomes nicer. It is a very nice surprise introduction in WWDC19.

All reactive framework has the same concepts, but differ slightly in implementation.

From here on, we will only talk about Apple's Reactive framework. Most part it is similar to the other frameworks, with the same name.

But **Combine** is a new name they have come out with. It could be a pun on "binding".

## The Problem

There are too many software patterns: KVO, delegate, block completion, notfication, etc..

But they are all not that nice.

## The components

1. Publisher
2. Subscriber
3. Operator

Publisher -> Operator A -> Operator B -> Subscriber

Operator basically take from upstream (publisher), do something with the value, and republish them downstream.

Finally, there is a subscriber.

> At the end of a chain of publishers, a Subscriber acts on elements as it receives them. Publishers only emit values when explicitly requested to do so by subscribers. This puts your subscriber code in control of how fast it receives events from the publishers it’s connected to.


## Subject

> A subject is a publisher that you can use to ”inject” values into a stream, by calling its `send()` method.

`PassthroughSubject`, `CurrentValueSubject`

## Scheduler
