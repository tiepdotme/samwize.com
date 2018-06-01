---
layout: post
title: "How my iOS Code evolved in Last 10 Years"
date: 2018-06-01T07:21:02+08:00
categories: [iOS]
---

I released my first iOS app in 2008, the year that iPhone changed the world.

Recently, in time for World Cup 2018, I release v3 of the [same app](http://just2us.com/sgfootball/).

As I rewrite the app -- from Objective-C to Swift -- it dawned on me how much things have changed.

How much the tools have improved. How different designs and architecture are. How much my code has evolved.

As I look at the code evolving from **v1 -> v2 -> v3** (thanks source control), I feel like reading my diary. It tells my story in iOS development.

## v1 (2008)

- SVN Source Control (git not popular back then)
- AppDelegate is 1,600+ lines long!
- Code form is inconsistent
  - Multiple line breaks
- Commented out many code for _some_ reason
- Table view cell are constructed with nibs, then configured with `viewWithTag`
- Models are dictionaries and arrays
- 1 month to finish

## v2 (2014)

- iOS 6 is a big change with cleaner interface
- Wrote better abstraction
  - Replace `Type1ViewController`, `Type2ViewController`, ... etc with `OddsViewController`, which configure the cell according to the type
- Still Objective-C (no Swift yet)
- Use of Cocoapods libraries
- Use of my own private library
- Storyboard
- Helpful models, but with lots of mutation func

## v3 (2018)

- Objective-C to Swift
  - Strongly typed language eliminates many crashes. Almost no crash.
- Better namings of everything
  - Xcode now can rename methods and types
- Scalable UI
  - Auto Layout for any device size or orientation
  - Dynamic (Font) Type
- No more storyboard
  - All UI created by code with constraints
- Better Architecture
  - Clearer responsibilities
  - Reasonable dependencies
  - MVVM with RxSwift
- Unit Tests with API fixtures
- Fastlane for managing certs, provisioning profiles, building and uploading releases
- 7 days to rewrite
