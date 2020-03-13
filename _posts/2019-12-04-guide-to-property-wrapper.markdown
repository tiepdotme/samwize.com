---
layout: post
title: "Guide to Property Wrapper"
date: 2019-12-04T16:38:11+08:00
categories: []
---

With [SE-0258](https://github.com/apple/swift-evolution/blob/master/proposals/0258-property-wrappers.md), Swift 5.1 adds a new [syntactic sugar](https://en.wikipedia.org/wiki/Syntactic_sugar).

Properties can now add `@SomeAwesomeAttribute`, which will wrap the property with _some more code_.

## Should you use it?

Some developers swear never to use `didSet` and `willSet` on properties, because that's just adding another place where logics will happen. Another place to look for bug. Another place that can easily break tests..

Similarly, you can avoid using property wrapper. Or at least you can **avoid creating your custom ones**.

If you use Combine or SwiftUI, then there is no escape from knowing them.

They have good reasons to use it. Some codes can be very repetitive; doing the same thing on a certain property. That's where a wrapper will make the code much readable.

## Example of a custom property wrapper

So if you're certain a property wrapper will make your code better, here's how to create one.

```swift
@propertyWrapper
struct Loud {

    private var original: String

    // 1. This is how the wrapper get & set the property
    var wrappedValue: String {
        get { return original.uppercased() }
        set { original = newValue }
    }

    // 2. An init
    init(wrappedValue: String)  {
        original = wrappedValue
    }

    // 3. Optional - provide another wrapper value of ANY TYPE, when access via the prefix `$`
    var projectedValue: Int {
        get { return original.count }
        set { original = "\(newValue)" }
    }
}
```

The code creates a `@Loud` that can wrap `String` properties, making them always UPPERCASED. You can also wrap generics, which I will not go into.

The essence:

1. `@propertyWrapper` declaration
2. `wrappedValue` with the type it can wrap
3. `init(wrappedValue:)` with a custom initialization
4. `projectedValue` (optional) is another special wrapper

## Usage

Let's wrap the `greetings` property with our custom wrapper `@Loud`.

```swift
struct User {
    @Loud var greetings = "Hello"
}
```

With that, using the property `greetings` will provide the wrapped value.

```swift
print(user.greetings) // "HELLO"
```

You can also use the **projected value** via `$greetings` (prefix with a `$`). In our custom property wrapper, it returns the string length, as an `Int`.

```swift
print(user.$greetings) // 5
```

A projected value is just _another convenient wrapper_, for _any type_.

In Combine, `@Published` uses the projected value to access the `Publisher` for the property. _So $ gives Publisher._ It is up to the custom property wrapper on _what $ gives_.

Since the `$` prefix don't give any meaning on what it actually projects, you have to dig into the code, or _RTFD_.

So once again, use them sparingly.

## BONUS: @dynamicMemberLookup

In [WWDC 2019, session 415](https://developer.apple.com/videos/play/wwdc2019/415/), along with property wrapper, they also talked about a **convenient key path member lookup**.

It is nice to understand how the framework provided `@State` and `@Binding` works.

![](/images/property-wrapper-dynamic-lookup.jpg)

`$slide.title` is slick, because the compiler actually rewrites to `$slide[dynamicMember: \Slide.title]`, which is then a `Binding<String>`!
