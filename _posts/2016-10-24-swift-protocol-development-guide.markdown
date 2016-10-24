---
layout: post
title: "Swift Protocol Development Guide"
date: 2016-10-24T15:43:06+08:00
categories: [Swift]
---

Not too long ago I briefly mentioned the use of protocol as the way of [Protocol Oriented Programming](http://samwize.com/2016/08/11/swift-is-a-protocol-oriented-programming-language/).

Protocol is fantastic.

Here is a guide on some of the common use of this Swift "feature".


## Declaring a Protocol 

Protocol are likes classes. You need to give it a name. 

You will commonly find names with **-able** suffixes eg. `TextRepresentable`.

Let's use a make up example of `Codable`, a protocol with a single function `code`:

```swift
protocol Codable {
  func code(text: String) -> Bool
}
```

Any class now that extends `Codable` will require the `code` method.


## Default Extension

Having a default `extension` makes things very convenient to use.

For example, the default implementation below will always return true.

```swift
extension Codable {
  func code(text: String) -> Bool {
    return true
  }
}
```

Now, any type that implements the `Codable` protocol get the default implementation, for FREE!

Of course, you may still implement your own (and not use the default).


## Default Extension with Type

Now, suppose you want the default extension to use a `UIViewController` method. 

You can enforce that the default extension be of the type `UIViewController`.

```swift
extension Codable where Self: UIViewController {
  func code(text: String) -> Bool {
    return self.view.hidden   // self is now a UIViewController!
  }
}
```

Of course, now any type that implements the `Codable` protocol must be a `UIViewController`, if it wants the default implementation. 


## Protocol of class type

You may enforce that a protocol is to be use by classes only (sorry structs and enums).

```swift
protocol Codable: class { ... }
```

This is needed especially for the scenario where you need a protocol to be `weak` (to avoid [retain cycle](http://samwize.com/2016/08/05/reference-cycle-for-closures/)). Because:

> weak can only be applied to class or class-bound protocol.


## Associated Types - Using `Self`

Think of associated types as a placeholder for an unknown type.

You can use `Self` in the protocol declaration to refer to the actual type that implements it.

```swift
protocol Codable {
  func code(foo: Self) -> Bool
}
```

So if later your `UIViewController` implements it, `foo` can be of the actual type.

```swift
extension MyViewController: Codable {
  func code(foo: MyViewController) { ... }
}
```


## Associated Types - Using `associatedtype`

You could in fact declare any generic type for a protocol.

Here, we declare `Code` as a generic type to be used in the protocol.

```swift
protocol Codable {
  associatedtype Code
  func code(foo: Code) -> Bool
}
```

Now, you can implement the actual type of `Code`. We use `Int` in this example:

```swift
class ActualCodable: Codable {
  func code(foo: Int) -> Bool {
    return foo > 10
  }
}
```

You can make the protocol [more readable](https://www.natashatherobot.com/swift-making-the-associated-type-parameter-readable-in-protocols/) if `foo` does not describes your parameter name well. In the protocol declaration, you can use `_` instead of naming it `foo`, then in implementation use any name you want.

Protocol is powerful :)
