---
layout: post
title: "Clean Code"
date: 2016-09-04T15:35:44+08:00
categories: [Coding]
---

Learning how to write clean code is a level every professional developer should go for.

[Clean code](https://cleancoders.com) by Uncle Bob, is an instructional video guiding you to writing better codes.

Here's what I learnt.


## Don't Write Comments

Don't be surprised. This is most important.

If you are writing comments,then you are not naming your functions/variables well enough.

Spend the time to name them concisely.

Spend the time to improve the design.

Comments are often carelessly neglected when code is updated. It is part of your "code" that becomes a burden, with outdated information. 

Reading and thinking over a comment wastes your energy. Is the comment valid? Now that I read the comment, let's read the method signature.. Comment is a waste of energy.

_Trivial: This is another example of [3 stages of life](http://just2me.com/2015/08/09/life-3-stages/)._ 

1. When we first learn coding, we didn't write comments.
2. Then to help others to understand, we write comments.
3. Then we don't write comments.


## Encapsulate till you drop

Break down your methods to a few lines, encapsulate


## Architecture

There is no clear definition of what is an architecture.

Often we quote architectures such as MVC, MVVM, VIPER, etc.. These are architecture defining how the layers (data, business, presentation, etc) work together.

But a architecture should really just look like the app it is.

How does the architecture of an accounting looks like? 

It shouts accounting when you look at it.
 

## Decomposition

Many design patterns, and decomposition is one of the most important.

It is actually a very simple concept.

Break down (decompose) a large component into smaller components.

Eg. The View Model (VM) in MVVM is a decomposition of MVC.

Encapsulate till you drop.
