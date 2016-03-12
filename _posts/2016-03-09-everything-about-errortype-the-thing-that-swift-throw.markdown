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

It works, but isn't cool.

_A bit of History: In the very beginning (in MacOS, way before any iOS), there is [NSException](https://www.bignerdranch.com/blog/error-handling-in-swift-2/) and try-catch in Objective-C, but eventually NSError took over._


## ErrorType

Then comes `ErrorType` in Swift.

It is merely a `protocol`.

And it does NOT have any method. It is an EMPTY PROTOCOL.

That means you can make any of your type a `ErrorType`, and have it being throw around.

It is said that subclass of `ErrorType` can be cast to `NSError`. It's true, because `NSError` has "implemented" the empty protocol. You can cast to `NSError` without any problem, but it don't help because you [lose information](http://stackoverflow.com/a/31685457/242682).

If you choose to use `ErrorType`, forget about `NSError`. Unless the `NSError` comes from other frameworks/libraries, then you might want to handle and map to one of your custom `ErrorType` case. Or you can wrap the NSError in your custom `ErrorType` (see `.OtherNSError` below).


## Custom ErrorType

Every app should have one (or more) custom `AppError` that subclass `ErrorType`.

It will usually be an enum (but you can use [class and struct](https://realm.io/news/testing-swift-error-type/) too, if you want), with different cases of errors, and init with different parameters (thanks to enum)!

```swift
enum AppError: ErrorType, CustomStringConvertible {
    case DivisionError
    case NetworkError(code: Int)
    case UnexpectedError(message: String)
    case OtherNSError(nsError: NSError)

    var description: String {
        switch self {
        ...
        }
    }
}
```

It will be good to implement `CustomStringConvertible`, and return `description` accordingly for each error case.


## try-catch-throw

I will not discuss more on how try-catch-throw works, because there are [many](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/Swift_Programming_Language/ErrorHandling.html), and I love this version:

[{%img center http://alisoftware.github.io/assets/frozen-throw-man.jpg %}](http://alisoftware.github.io/2015/12/17/let-it-throw/)

[Let it throw, let it throw~](http://alisoftware.github.io/2015/12/17/let-it-throw/)


## An Example

But I will provide an example using [Unbox library](https://github.com/JohnSundell/Unbox) (a good JSON decoder) of how to catch it's `UnboxError` (their custom `ErrorType`):

```swift
do {
    ...
} catch UnboxError.MissingKey(let key) {
    // Specific error case of a MissingKey
    print("Missing Key", key)
} catch let error as UnboxError {
    // Catch all other UnboxError
    // Because UnboxError conforms to CustomStringConvertible, we can print `description`
    print(error.description)
} catch let error as NSError {
    // Warning: Cast to NSError (details will be lost). Not recommended.
    print(error.localizedDescription)
}
```


## Handling throw in closures

There are 2 ways as discussed in [appventure.me](http://appventure.me/2015/06/19/swift-try-catch-asynchronous-closures/):

1. Using `ResultType`
2. Using inner closure that throws

Using `ResultType` is simpler, and easier to read. It became popular as more developers use the `ResultType`, even though it is not in Swift standard library.
