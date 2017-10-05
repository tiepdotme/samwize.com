---
layout: post
title: "Access Levels in Swift"
date: 2017-04-20T11:31:44+08:00
categories: []
---
 
![The 5 Access Levels in Swift](/images/swift-access-levels.jpg)

## The Default

The default is `internal`, which means access is restricted to within a module. 

What is a module? An app is 1 module.

Hence, the default for an app is everything is accessible within the app.

## Which to use?

The good practise is to start being extremely restrictive.

Start with `private`, and expose more, only if necessary. 

## Application vs Framework Development

For regular application development, you will use only (1) to (3).

For developers working on framework/library/SDK, they will use (4) and (5), because their "module" is exposed to other developers. The difference between the last 2 levels is that `public` does not allow the type/func to be subclassed/overriden, while `open` let you do whatever you want.

## Implicit

If an access level is omitted, it will be implicitly the default -- `internal`

```swift
public class X {
    let i = 1   // implicitly internal
}
```

## Specify explicitly for top-level definitions

A [good practise](https://github.com/github/swift-style-guide) is to specify the access level explicitly for the top-level types and functions. 

Don't leave it to the default (`internal`). Think hard if you need other part of your app to access it.

## Testing

    @testable import MyApp

In your unit tests, you can import with `@testable` attribute, which is a superpower to [change access levels](https://developer.apple.com/library/content/documentation/DeveloperTools/Conceptual/testing_with_xcode/chapters/04-writing_tests.html) in the module/app, so that in your tests you can access them.

For example, an `internal` class is not accessible to test target, because a test target is an external module. With `@testable`, the access level is increased to `open`, and you can now access it (and may even subclass it)!

## Final

Finally, another good practise is to lock down your definitions with `final` (:

This attribute provides an additional restriction -- prevent others from subclassing and overriding it.

## Wait. Lastly, it's hell of a mess from Swift.

This is a complain on how access levels have evolved, yet is still not great in Swift 3.

In Swift 1, there are 2 levels.<br />
In Swift 2, there are 3 levels.<br />
In Swift 3, there are 5 levels.<br />

I believe it is a mistake in Swift 3 to adopt [SE-0025](https://github.com/apple/swift-evolution/blob/master/proposals/0025-scoped-access-level.md). 

My biggest gripe is this: I have extension X to a type T, keeping them in seperate files. If I want X to use a private member in T, it is impossible to do so with `private` or `fileprivate`. I am forced to increase the access to `internal`, which is not what I want. 

There is no way to have type level private. The current `private` is local scope private, and `fileprivate` is a [weird brother](https://lists.swift.org/pipermail/swift-evolution/Week-of-Mon-20170403/034903.html) that extends to within that file.

After Swift 3 was introduced, there is proposal [SE-0159](https://github.com/apple/swift-evolution/blob/master/proposals/0159-fix-private-access-levels.md) to fix the mistake, but was [rejected](https://lists.swift.org/pipermail/swift-evolution/Week-of-Mon-20170403/034902.html). Clearly the core team acknowledged the shortcoming, but instead of changing the keywords again, they will likely introduce a "Type-based" `private` access in Swift 4.
