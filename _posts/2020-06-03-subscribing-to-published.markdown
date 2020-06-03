---
layout: post
title: "Subscribing to @Published"
date: 2020-06-03T12:29:31+08:00
categories: [SwiftUI, Combine]
---

I wrote on [StackOverflow](https://stackoverflow.com/q/56735382/242682) with a piece of code on how to observe an `@Published`.

```swift
class MyValue: ObservableObject {
    @Published var value: String = ""
    init() {
        $value.sink { ... }
    }
}
```

The code works, but might not be as intended.

In the sink, if you print out `value`, it will NOT be the new value. It will be the old value, before the change.

The subscriber is getting the sink when the value `willChange`, NOT `didChange`.

That's how `ObservableObject` [works](https://developer.apple.com/documentation/combine/observableobject/3362556-objectwillchange). But in SwiftUI early beta, it was once designed to emit after the change, with `objectDidChange`.

## Why SwfitUI emit BEFORE the change?

Why did the design changed?

It was discussed [here](https://forums.swift.org/t/combine-observableobject-in-uikit/28433/2). In this [tweet](https://twitter.com/luka_bernardi/status/1151633281982406656) they mention the reason:

> We made the change because it allows us to do a better job in coalescing updates; especially in the presence of animations.

For the sake of them doing a _better job_, optimizing for animations, they made the change.

It is a pitfall, so we need workarounds.

## Solution 1

The easiest way is to use a dispatch call.

```swift
$value.sink { [weak self] in
    DispatchQueue.main.async { [weak self] in
        ...
    }
}
```

That _usually works_.

## Solution 2

The more full proof way is to have a separate event being emitted when the `@Published` is `didSet`:

```swift
class MyValue: ObservableObject {
    var valueDidChange = PassthroughSubject<Void, Never>()

    @Published var value: String = "" {
        didSet {
            valueDidChange.send()
        }
    }
}
```

Then subscribe to `valueDidChange` instead.

```swift
$valueDidChange.sink { [weak self] in
    ...
}
```
