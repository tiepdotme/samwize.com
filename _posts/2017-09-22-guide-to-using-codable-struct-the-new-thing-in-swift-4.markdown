---
layout: post
title: "Guide to Using Codable Struct for JSON"
date: 2017-09-22T12:30:24+08:00
categories: [Swift]
---

http://samwize.com/2017/08/03/wwdc-2017-whats-new-in-foundation/

https://developer.apple.com/documentation/swift/codable
https://developer.apple.com/documentation/foundation/archives_and_serialization/encoding_and_decoding_custom_types

The code -- open source yeah
https://github.com/apple/swift/blob/master/stdlib/public/core/Codable.swift#L3368

Good guide
http://swiftjson.guide
https://www.raywenderlich.com/172145/encoding-decoding-and-serialization-in-swift-4

https://developer.apple.com/documentation/foundation/archives_and_serialization/using_json_with_custom_types

Progressively more advanced

## The Basic

```swift
struct Animal: Codable {
    var numberOfLegs: Int
}
```

Add `Codable` as a trait to your type, and that's it. You enjoy the **automatic encoding and decoding** thanks to default extension for the protocol.

It works automatically as long as the members are `Codable` type. Later section will explain what to do if they are not supported.

`Codable` is actually made up of 2 protocols -- `Encodable` and `Decodable` -- and you can just use 1 of you don't need the other. In this post, we will highlight for both encoding and decoding, because usually you will need both.

## Encoding to JSON string

With a `Codable` type you can encode to JSON string easily.

```swift
let animal = Animal(numberOfLegs: 4)
let encoder = JSONEncoder()
let data = try! encoder.encode(animal)
print(String(data: data, encoding: .utf8)!)
```

If you want pretty print, add `encoder.outputFormatting = .prettyPrinted`.

## Decoding from JSON string

```swift
let decoder = JSONDecoder()
let jsonData = jsonString.data(using: .utf8)!
let animal = try! decoder.decode(Animal.self, from: jsonData)
```

## When you need a different key name

Let's say for the JSON, you want the key name to be "number_of_legs" (snake cased).

If you want to change the JSON key name, you can add add `CodingKeys` enum to the struct.

```swift
struct Animal: Codable {
    var numberOfLegs: Int
    enum CodingKeys: String, CodingKey {
        case numberOfLegs = "number_of_legs"
    }
}
```

Now, there is some magic performed by the compiler with the `CodingKeys` enum. The compiler only recognize the name "CodingKeys", reserved as the keys for the struct. 


## When you need a nested structure

When your struct is flat, but maps to a nested structure in the JSON, you have more work to do.

```swift
struct Animal {
    var numberOfLegs: Int
    
    enum CodingKeys: String, CodingKey {
        case anatomy
    }
    
    // The key for the nested structure in the JSON
    enum AnatomyCodingKeys: String, CodingKey {
        case numberOfLegs
    }
}

extension Animal4: Encodable {
    func encode(to encoder: Encoder) throws {
        var container = encoder.container(keyedBy: CodingKeys.self)
        var anatomyContainer = container.nestedContainer(keyedBy: AnatomyCodingKeys.self, forKey: .anatomy)
        try anatomyContainer.encode(numberOfLegs, forKey: .numberOfLegs)
    }
}
```


## Error with a member Dictionary

Let's look at an unexpected scenario, a struct having a Dictionary as it's member. 

```swift
struct Sword: Codable {
    var properties: Dictionary<String, Codable>
}
```

`Sword` has a flexible member `properties`, which basically can store any key-value pair. But there will be a compile error.

    Type 'Sword' does not conform to protocol 'Encodable'
    Type 'Sword' does not conform to protocol 'Decodable'
    
Requires dynamcing coding keys.

## Dynamic Coding Keys

This is complex.

See playground.

## What is a container?

A container can be one of a few different types:

1. Keyed Container – provides values by keys. This is essentially a dictionary.
2. Unkeyed Container – this provides ordered values without keys. In the JSONEncoder, this means an array.
3. Single Value Container – this outputs the raw value without any kind of containing element.

Steps to encoding/decoding:

1. Get the container (one of the 3 type)
2. Encode/Decode 
