---
layout: page
title: "Swift Advanced Concepts"
permalink: /development/swift-advanced/
---

# Swift Advanced Concepts

## Protocol Compostion

You can use `&` to [compose](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Protocols.html#//apple_ref/doc/uid/TP40014097-CH25-ID282) multiple protocols.

[Dependency injection](http://merowing.info/2017/04/using-protocol-compositon-for-dependency-injection/) with the same `init` that can add new dependency in the future. The trick is to group multiple dependencies in one simple data struct.
