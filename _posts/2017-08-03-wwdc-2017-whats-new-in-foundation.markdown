---
layout: post
title: "WWDC 2017 - What's New in Foundation"
date: 2017-08-03T16:17:08+08:00
categories: [Swift]
---

Summary of what's new in Foundation, Swift 4, from [session 212](https://developer.apple.com/videos/play/wwdc2017/212/). _Tip: You can download WWDC videos easily with [wwdc-dl](https://github.com/samwize/wwdc-dl)._

## Key Paths

Apple calls it Smart, because it is statically safe, and fast.

It is built into Swift language, with a single `\`

![](/images/wwdc-foundation-keypath.png)

```swift
// To get via keypath
let age = ben[keyPath: \Kid.age]

// To set via keypaht
ben[keyPath: \Kid.nickname] = "Ben"
```
    
## Appending Key Paths

You can append key path, provided the type you "chain" is the same.

![](/images/wwdc-foundation-keypath-appending.png)

So if you append, the final keypath is simply of type `Keypath<BirthdayParty, Double>`.
  
There are more keypath types.

![](/images/wwdc-foundation-keypath-more.png)

## Key-Value Observation

![](/images/wwdc-foundation-keypath-kvo.png)

```swift
// To use
let observation = mia.observe(\.age) { observed, change in
  // observed is the updated "mia"
}
```

## Codable/JSON

Swift also finally recognize the importance of JSON. 

You simply add the trait protocol `Codable` to your model, and it will work (because of [default protocol extension](http://samwize.com/2016/10/24/swift-protocol-development-guide/)).

![](/images/wwdc-foundation-codable.png)

If you want to customize the key names, you can simply add your own `CodingKeys` as an enum in your model. See how the camel case "comment_count" is customized:

```swift
private enum CodingKeys : String, CodingKey {
    case author
    case commentCount = "comment_count"
}
```

Ok, actually `Codable` is not only for JSON data, but for [other formats](https://developer.apple.com/documentation/foundation/archives_and_serialization) like Property List too. There is `JSONDecoder`, and also `PropertyListDecoder`

## What Else ?

There is [more](https://www.raywenderlich.com/163857/whats-new-swift-4) in Swift 4, some small, some advanced API. Many were not touched in session 212.

Not going to cover all, but here are some teasers:

- String is now a Collection
- Multi-line string with `"""`
- Collection has more API such as `mapValues`, `MutableCollection.swapAt(_:_:)`, default Dictionary value
- Infer one-sided range eg. infer start and end index
- Generic subscript
- `fileprivate` [access level](/2017/04/20/access-levels-in-swift/) for extension across files
- [No more](/2017/09/20/migrating-to-swift-4-and-xcode-9/) @objc inference
