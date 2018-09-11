---
layout: page
title: "Core Libraries"
permalink: /libs/core/
---

## [Dependencies Injection](https://github.com/Swinject/Swinject)

One place to "inject" all the services that your app use.

## Attributed String

`NSAttributedString` is not that friendly. Checkout for a better world with [SwiftRichString](https://github.com/malcommac/SwiftRichString).

## JSON Decoder

- [ObjectMapper](https://github.com/Hearst-DD/ObjectMapper) - Using `<-` operator. Very popular, with [Alamofire extension](https://github.com/tristanhimmelman/AlamofireObjectMapper)
- [Unbox](https://github.com/JohnSundell/Unbox)
- [Decodable](https://github.com/Anviking/Decodable)

## Date Time

- [DateTools](https://github.com/MatthewYork/DateTools): Date time utility, and moments such as "4 seconds ago"
- [YLMoment](https://github.com/samwize/YLMoment) (my fork) - easy format, and moments based on [moments.js](http://momentjs.com)

But above 2 not complete enough. [SO](http://stackoverflow.com/questions/10075898/ios-friendly-nsdate-format) suggested others:

- [FormatterKit](https://github.com/mattt/FormatterKit) by matt (popular): Not only for formatting dates, but also addresses and location
- [NSDate+NVTimeAgo](https://github.com/nikilster/NSDate-Time-Ago): Facebook style

## Logging

- [CocoaLumberjack](https://github.com/CocoaLumberjack/CocoaLumberjack)
- [SwiftyBeaver](https://github.com/SwiftyBeaver/SwiftyBeaver)

## Encryption

- [Lockbox](https://github.com/granoff/Lockbox): save to keychain easily
- [Heimdall](https://github.com/henrinormak/Heimdall): He is the gatekeeper at the rainbow road (in Thor). Simple encrypt, decrypt and signing of text.
- [KeychainSwiftAPI](https://github.com/deniskr/KeychainSwiftAPI)

## Core Data

- [Sync](https://github.com/hyperoslo/Sync) sync JSON into Core Data
- [CoreStore](https://github.com/JohnEstropia/CoreStore) in Swift 2, very well documented, and seems complete, and even have a [different/better stack design](http://floriankugler.com/2013/04/29/concurrent-core-data-stack-performance-shootout/) than MagicalRecord. But they have "transactions" instead of predicates.

## Autolayout

- [Masonry](https://github.com/Masonry/Masonry): High level approach, but deprecated for Objc. The new swift version is known as [SnapKit](https://github.com/SnapKit/SnapKit).
- [PureLayout](https://github.com/smileyborg/PureLayout): Provide handy APIs to UIView
- [Cartography](https://github.com/robb/Cartography): Declarative style

## Unit Testing

- [OHHTTPStubs](https://github.com/AliSoftware/OHHTTPStubs) - stub for networking frameworks

## Avoid String Identifiers

- [Reusable](https://github.com/AliSoftware/Reusable) - Avoid cells, xib views, storyboard view controller's string identifier with this convenient mixin
- [SwiftGen](https://github.com/SwiftGen/SwiftGen) - Avoid more strings (eg. localization, assets) by generating code (like Android's R)
- [R.swift](https://github.com/mac-cain13/R.swift) - An actual R for Swift

## Parsers

- [Kanna](https://github.com/tid-kijyun/Kanna) - Parse XML/HTML, inspired by Nokogiri

## Deeplinks

- [Appz](https://github.com/SwiftKitz/Appz) - provides (as much) external apps deeplink schema as opening them is a breeze
