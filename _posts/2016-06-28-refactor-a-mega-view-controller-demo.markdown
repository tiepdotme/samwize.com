---
layout: post
title: "Refactor a Mega View Controller - A Live Coding!"
date: 2016-06-28T17:06:36+08:00
categories: [Architecture]
---

Andy Matuschak gave a good talk on refactoring a mega controller. 

[Watch it](https://realm.io/news/andy-matuschak-refactor-mega-controller/).

[![](/images/refactor-a-mega-view-controller-demo.png)](https://realm.io/news/andy-matuschak-refactor-mega-controller/)

It is a very good video by Andy Matuschak,
performing delightful live coding
and narrating his thoughts process.

You might recognize Andy,
an ex-Apple engineer 
who gave a great talk in [WWDC 2014](https://developer.apple.com/videos/play/wwdc2014/229/)
on app architecture -- __where is the truth__?


## The Usual Problem

We often got into
is writing Massive View Controller,
resulting in 1 single file that contains all kind of code!

Apple sample code 
and programming guide
didn't help to guide us towards clean code/architecture.


## Techniques

There is no magic.

> Mostly Decomposition

Or as Uncle Bob says: _Extract till you drop_

> Where is the truth?

Think hard which class should hold the truth.

> It is VERY RUDE for a view controller to change it's parents properties

For example, don't change your navigation bar tint color directly. Your view controller does not own the navigation bar. What if another (child) view controller also change the tint?

Test driven development (TDD) will force you
to architect your code nicely.


## What Pasta is Your Code?

As you decompose/abstract/encapsulate your code, the structure changes.

1. Spaghetti code - messy
2. Lasagna code - well layered
3. Ravioli code - very much decoupled

![](http://www.proun-game.com/Oogst3D/BLOG/Italian%20Food%20Coding%20Ravioli.jpg)

It is up to you to find the balance,
which is not easy.

You want to decompose as much, 
but not to the extend
that it difficult to follow the flow of data.


## Bonus

Storyboard instantiate your initial view controller
so you have no chance to init yourself.

A trick is shown in the talk
where you instantiate your VC manually in `AppDelegate`
and set it to the navigation VC (the root VC of window).
