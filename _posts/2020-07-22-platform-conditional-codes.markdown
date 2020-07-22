---
layout: post
title: "Platform Conditional Codes"
date: 2020-07-22T21:45:49+08:00
categories: []
---

Swift has provided us quite a few ways to write codes that work for specific platform, version, and language. Examples:

- _Run code only for only iOS 13_
- _Run code only for Swift 5_
- _Run code only if can import SwiftUI_

## #available

This is a **runtime check**, so you can use in regular conditional statements.

```swift
if #available(iOS 13.6, macOSApplicationExtension 10.15, *) && someOtherBoolean {
    ...
} else {
    // Fallback code
}
```

Check out the [grammar](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar_availability-condition) for details.

The trailing `*` denotes that it is available for all other platforms, so that future ones such as `glassOS` will be supported, when release.

## @available

This is an [attribute](https://docs.swift.org/swift-book/ReferenceManual/Attributes.html) that applies to types and properties. The grammar is same as `#available`.

```swift
@available(iOS 12, *)
struct My12Monkeys { ... }
```

You can also specify for language version:

```swift
@available(swift 5) // NOTE: Must be a lower case 's'
func swift5Only() { ... }
```

You can also annotate more info like this:

```swift
@available(iOS, deprecated: 13, message:"No reason", renamed: "Singapore")
struct Singapura { ... }
```

You can also stack multiple attributes.

## canImport()

This [tests for modules](https://github.com/apple/swift-evolution/blob/master/proposals/0075-import-test.md) availability.

```swift
#if canImport(SwiftUI)
    // SwiftUI code
#else
    // Fallback to UIKit
#endif
```

### Related

- [Preprocessor Codes for Swift](/2019/02/26/preprocessor-codes-for-swift/)
