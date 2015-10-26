---
layout: post
title: "Pitfall: Swift Variadic Parameters and Array"
date: 2015-10-21T17:33:25+08:00
categories: [Swift]
---

I was using `String` format with variadic parameters, and was very puzzled by how it works.

See this piece of playground code:

```swift
let f = "Hello %d World %d"

// Pass [1, 2] directly
String(format: f, arguments: [1, 2])
// Prints "Hello 1 World 2" correctly

// Now, lets create an array [1, 2], and pass in
let arr = [1, 2]

// Surprisingly, this will CRASH
String(format: f, arguments: arr)
```

This puzzled me.

Looks so similarly, yet one will crash and burn.

## Swift Type

Apparently, arguments require the type `[CVarArgType]`, and if you will to declare it like `arr` as above, the type will be `[Int]`, and that's not ok.

[This thread](https://devforums.apple.com/message/970958) shed light on why the array is not cast/unpacked automatically.

> Should the compiler choose T=Int or T=Int[] ?
> Swift's generics system allows T to be any type, including an array type, so we don't allow automatic "unpacking" of arrays when calling into variadics, meaning that we'll always pick T=Int[]. There should be some way to explicitly unpack an array (or any sequence?), and we (internally) have a Radar requesting this feature.

The fix is to explicitly provide the type:

```swift
let arr: [CVarArgType] = [1, 2]
String(format: f, arguments: arr)
```

