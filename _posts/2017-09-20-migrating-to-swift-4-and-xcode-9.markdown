---
layout: post
title: "Migrating to Swift 4 and @objc"
date: 2017-09-20T16:37:53+08:00
categories: [Swift, Xcode]
---

Swift's official [migration guide](https://swift.org/migration-guide-swift4/) gives a good overview.

Using the migration assistant in the new Xcode 9 would have helped to rename most of the SDK changes, such as moving string contants to enum cases.

This post will highlight the other trickly cases.

## Distinguish single-tuple from multiple-argument function types [SE-0110]

There is an issue with Swift 3 in distinguishing between the type of the argument in a closure.

Is it a tuple with 2 arguments?

Or is it 2 arguments?

Proposal [SE-0110](https://github.com/apple/swift-evolution/blob/master/proposals/0110-distingish-single-tuple-arg.md) seeks to correct that. 

In short, to declare a function type with 1 tuple, you will need to double enclose with brackets:

```swift
let a : ((Int, Int, Int)) -> Int = { x in return x.0 + x.1 + x.2 }
```

Xcode will also suggest rewriting `f: (Void) -> ()` to `f: () -> ()`, because your probably just mean that.

## @objc has to be explicit [SE-0160]

The biggest change is to do with Objective-C inference.

The details is in the proposal [SE-0160](https://github.com/apple/swift-evolution/blob/master/proposals/0160-objc-inference.md) (it's long).

The change is not only source breaking, but worse, could introduce bugs unknowingly. This is because you could still build and run, but if some of your type/method is no longer available to Objective-C runtime (because @objc is no longer inferred anymore), the method will just fail to respond.

That's why this change is big, and you should test all parts of your user flow.

#### The switch in project settings

After migrating to Swift 4, **Swift 3 @objc Inference** remains **On**, for good reason because as we said, this change is breaking.

There is benefits to switching it to off -- that is not to infer @objc for all types. Things will be faster, binary smaller.

When you switch to **Off** (default), make sure your app still works well. You could unknowingly introduce bugs (likely not crash) because some functions are no longer inferred to be available to Objective-C runtime.

#### When it doesn't work..

To fix, you can add 2 annotations to tell the compiler it is to be available for Objective-C runtime.

`@objc` can be added to a function.

`@objcMembers` can  be added to a type, and all members and functions will be infered, unless, of course, if the function uses pure Swift features eg. tuple.

#### Extensions cannot override yet

Another common error as a result is to do with extension methods that are overriden.

```swift
extension Super {
  func foo() { } // Remember, without @objc, this is not available to objc 
}

class Sub : Super {
  override func foo() { }
}
```

The above code will have error "declarations in extensions cannot override yet".

The fix is to add @objc to the function in the extension.

#### Summary of code impact

- Subclass of `NSObject` no NOT infer as @objc
- Run your tests
- Check all app flow to make sure they are working, if not add `@objc` and `@objcMembers`
- Add @objc to extension functions that needs to be overriden
- `dynamic` must have @objc -- this is easy to fix because compiler will have error
