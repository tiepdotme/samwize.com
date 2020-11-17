---
layout: post
title: "Exponential Power in Swift"
date: 2020-11-13T11:18:37+08:00
categories: [Foundation]
published: false
---

Do you know how to represent exponential power in Swift?

I thought it is using the `^` operator.

If you think so too, you better read on. I failed a coding interview question because I assumed so.

## What is `^` operator?

After I finish the interview, I investigate why my solution is always wrong. Opening Xcode playground, I Alt-click on the `^`.

> Returns the result of performing a bitwise XOR operation on the two given values.

Oops. It is XOR. The last time I use XOR was long time ago in my engineering class working on the logic board.

## What is the exponential power operator?

Turns out, in Foundation framework, there is a [`pow(_:_)`](https://developer.apple.com/documentation/foundation/1779833-pow).

The first argument has to be a `Decimal`. So if you have an Int, you could conveniently cast it to Double.

```swift
pow(Double(anInt), powerInt)
```

_Anyway, I did use pow before donkey years ago, and that was probably a rare opportunity to use it in an app._

## What is Decimal?

In case you didn't know, `Decimal` is a more precise type to represent numbers. You will need it in financial calculations so as to represent money precisely (you won't want the system to round your money)!

Decimal uses more bits than Double, and will be slower.

It can represent numbers precisely because it stores a number as 3 components: the sign, significant, and exponent.
