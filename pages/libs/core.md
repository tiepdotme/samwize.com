---
layout: page
title: "Core Libraries"
permalink: /libs/core/
---

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

- [/hyperoslo/Sync](https://github.com/hyperoslo/Sync): Takes JSON sync into Core Data
- [CoreStore](https://github.com/JohnEstropia/CoreStore): Swift 2. Very well documented, and seems complete, and even have a [different/better? stack design](http://floriankugler.com/2013/04/29/concurrent-core-data-stack-performance-shootout/) than MagicalRecord. But they have "transactions" instead of predicates.


## Autolayout

- [Masonry](https://github.com/Masonry/Masonry): High level approach, but deprecated for Objc. The new swift version is known as [SnapKit](https://github.com/SnapKit/SnapKit).
- [PureLayout](https://github.com/smileyborg/PureLayout): Provide handy APIs to UIView
- [Cartography](https://github.com/robb/Cartography): Declarative style


## Unit Testing

- [OHHTTPStubs](https://github.com/AliSoftware/OHHTTPStubs) - stub for networking frameworks

