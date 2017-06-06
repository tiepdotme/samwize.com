---
layout: post
title: "Tip: Debugging UI Calls in Background Threads"
date: 2017-06-06T09:52:50+08:00
categories: [iOS]
---

We all know UI calls have to be run in the main thread. 

But sometimes mistake happen, and you will see this in debug log:

> This application is modifying the autolayout engine from a background thread after the engine was accessed from the main thread. This can lead to engine corruption and weird crashes.

There is a easy way to trace which part of your code is casuing the problem.

1. Add Symbolic Breakpoint
2. Symbol: `[UIView layoutIfNeeded]` 
3. Condition: `!(BOOL)[NSThread isMainThread]`

There are other "symbols" to catch. Create a symbolic breakpoint for **each of these symbols**:

- `[UIView updateConstraintsIfNeeded]`
- `[UIView setNeedsUpdateConstraints]`
- `[UIView setNeedsDisplay]`
- `[UIView setNeedsLayout]`

Run the app, and Xcode will break at the offending code.

Lastly, the fix as we all know:

```swift
DispatchQueue.main.async {
    // The UI call
}
```