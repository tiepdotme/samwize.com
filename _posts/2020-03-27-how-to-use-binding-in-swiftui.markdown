---
layout: post
title: "How to use Binding in SwiftUI"
date: 2020-03-27T16:01:43+08:00
categories: [SwiftUI]
---

`@State` and `@ObservedObject` are used to declare the source of truth.

On the other hand, `@Binding` is **NOT the truth**, but an intermediary, or kind of derived truth.

## Binding to a state

We can declare a state easily in a view, for a **value type** (never a reference type).

```swift
@State private var title: String
```

We can use the `title` as per normal String, and access it's binding via `$title` ($ will use the projected value of [property wrapper](/2019/12/04/guide-to-property-wrapper/)).

```swift
// Usage 1: Providing the value to a view
Text(title)

// Usage 2: In a 2-way binding
TextField($title)
```

Binding is like providing a **reference type** to the actual value type. And it is 2-way, such that when the `TextField` set the binded value, the actual value type will be updated. It is kind of _like passing a reference_ to the TextField.

## Declare a binding

Let's say now you have a view that does not have the source of truth, and so you gonna provide a binding. Declaring the binding type is easy.

```swift
@Binding var title: String
```

The usage is exactly the same as using state.

- `$title` is still the binding, of type `Binding<String>`
- `title` is still a `String` type

## Providing a binding init

You can provide a custom init and pass in `Binding<String>`.

```swift
init(title: Binding<String>) {
    self._title = title
}
```

Just 1 strange syntax.

To set the binding, you have to use `_title` -- yes, with an underscore! Why, you may wonder..

## Isn't `$` used to access the binding?

Yes, `$title` will access the binding. But it is a **get-only property**, as said in the doc.

```swift
/// The binding value, as "unwrapped" by accessing `$foo` on a `@Binding` property.
public var projectedValue: Binding<Value> { get }
```

To **set** the binding, you have to access the **synthesized property** with a `_`.

Nostalgic.

_Reminds me of the magical Objective-C days.._

## Creating a binding

So far we use the property wrapper `@Binding` to wrap a value type, thus providing a binding.

We can also create an instance of a binding directly, providing the getter & setter implementations.

```swift
let title = Binding<String>(
    get: {
        // Provide the getter
        return "Hello World"
    },
    set: { s in
        // Provide the setter
    })
```

To get the String value of the binding, use `title.wrappedValue`.

## Constant Binding

The above getter implementation in fact is so simple, that you can use a `constant` "binding".

```swift
let helloWorld = Binding.constant("Hello World")
```

## Transforming a binding

We have the title `Binding<String>`. How do we implement a isTitleTooLong `Bool`? Answer: Computed property.

```swift
var isTitleTooLong: Bool { return title.count > 10 }
```

This is just a _computed property_, but it works for SwiftUI because when a dependency is updated, the whole view is evaluated.

You can also wrap the computed property with a `Binding<Bool>`.

You can even create a binding like this (in body, but [not in init](https://forums.swift.org/t/how-to-fix-error-escaping-closure-captures-mutating-self-parameter-in-init/29870/4)):

```swift
var body: some View {
    let reversed = Binding<String>(
        get: {
            return String(self.title.reversed())
        },
        set: { s in
            self.title = String(s.reversed())
        }
    )
    ...
}
```

However, in all these cases, when the title `Binding<String>` is updated, it does not directly emit the event to the computed property nor the transforming binding. Which brings us to the next section.

## Observing a binding

I don't know how to do that.. Tell me if you know 😉

Here's a thought, but it does not use Binding. Instead, use `ObservableObject` with `@Published` property (property wrapper providing a publisher). The published property can easily be observed.

```swift
observableObject.$publishedValue
    .sink { value in
        // Observe
    }
```
