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

If a type has a certain access level, the properties within will have the same level, implicitly.

```swift
private class X {
    int i   // implicitly private
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
