---
layout: post
title: "Everything About ErrorType - the Thing That Swift Throw"
date: 2016-03-09T22:24:15+08:00
categories: [iOS]
---

`ErrorType` is a big thing is Swift, changing the way how errors are handled.


## NSError is the Past

[`NSError`](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/Foundation/Classes/NSError_Class) was the way how errors are encapsulated, and passed along in methods and blocks/closures.

But it was not a good design.

It differentiates the errors using `code` (Int), and also `domain` (String).

And it passes more information in `userInfo` as a dictionary.

It works, but isn't cool


## ErrorType

Then comes `ErrorType` in Swift.

It is merely a `protocol`.

And it does NOT have any method. It is an EMPTY PROTOCOL.

That means you can make any of your type a `ErrorType`, and have it being throw around.

It is said that subclass of `ErrorType` can be cast to `NSError`. It's true, because `NSError` has "implemented" the empty protocl. You can cast to `NSError`, but it only makes sense if you really know that is an `NSError`.


## Custom ErrorType

Every app should have one (or more) custom `AppError` that subclass `ErrroType`.

It will be an enum, with different cases of errors.

```swift
enum AppError: ErrorType, CustomStringConvertible {
    case DivisionError
    case NetworkError(code: Int)
    case UnexpectedError(message: String)

    var description: String {
        switch self {
        ...
        }
    }
}
```

You could also implement `CustomStringConvertible`, and return `description` accordingly for each error case.


## try-catch-throw

I will not discuss more on how try-catch-throw works, because there are [many](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/ErrorHandling.html), and I love this version:

[{%img center http://alisoftware.github.io/assets/frozen-throw-man.jpg %}](http://alisoftware.github.io/2015/12/17/let-it-throw/)

[Let it throw, let it throw~](http://alisoftware.github.io/2015/12/17/let-it-throw/)


## Handling throw in closures

There are 2 ways as discussed in [appventure.me](http://appventure.me/2015/06/19/swift-try-catch-asynchronous-closures/):

1. Using `ResultType`
2. Using inner closure that throws

Using `ResultType` is simpler, and easier to read. It became popular as more developers use the `ResultType`, even though it is not in Swift standard library.
