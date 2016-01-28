---
layout: post
title: "Pitfall With Unwrapping Optional of an AnyObject"
date: 2016-01-28T16:09:17+08:00
categories: [iOS]
---

We all assume an **if-let** way of unwrapping optional works as it is.

Example:

```swift
let json: AnyObject = ["x": "y"]
if let foo = json["foo"] {
    print("Should not see this since json[\"foo\"] is nil!")
}
```

In the above, we should not see the print statement since `json["foo"]` is nil.

Yet, we will see the print!

This is because [AnyObject is evil](http://stackoverflow.com/a/28531068/242682). It will ruin things, run wild, and unpredictable.

To fix, you need to cast to the type that you know the unwrapped optional will be eg. String:

```swift
let json: AnyObject = ["x": "y"]
if let foo = json["foo"] as? String {
    print("Should not see this since json[\"foo\"] is nil!")
}
```
Now, you don't see the print!

_UPDATE: An easier way is_

```swift
if let foo = json["foo"] ?? {
```
