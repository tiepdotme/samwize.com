---
layout: post
title: "Guide to Using Codable Struct for JSON"
date: 2017-09-26T12:30:24+08:00
categories: [Swift]
---

`Codable` is a new feature introduced along with Swfit 4 for encoding and decoding models easily, making third party libraries such as SwiftyJSON and Unbox obsolete. It is part of Foundation framework, and is a must know if you use JSON format.

## The Basic

```swift
struct Animal: Codable {
    var numberOfLegs: Int
}
```

Add `Codable` as a trait to your type, and that's it. 

You enjoy **automatic encoding and decoding**, thanks to default extension for the `Codable` protocol.

```
{
    "numberOfLegs" : 2
}
```

It works automatically as long as the members are `Codable` type. Later section will explain what to do if your type cannot conform to `Codable`.

`Codable` is actually made up of 2 protocols -- `Encodable` and `Decodable` -- and you can use one if you don't need the other. In this post, we will highlight for both encoding and decoding, but feel free to decouple them.

## Encoding to JSON string

With a `Codable` type you can encode to JSON string easily.

```swift
let animal = Animal(numberOfLegs: 4)
let encoder = JSONEncoder()
encoder.outputFormatting = .prettyPrinted
let data = try! encoder.encode(animal)
print(String(data: data, encoding: .utf8)!)
```

You probably will omit the pretty print in production code.

## Decoding from JSON string

```swift
let decoder = JSONDecoder()
let jsonData = jsonString.data(using: .utf8)!
let animal = try! decoder.decode(Animal.self, from: jsonData)
```

## What about plist?

This post is about JSON, but it is trivial to encode/decode other formats.

Simply change `JSONEncoder`/`JSONDecoder` to `PropertyListEncoder`/`PropertyListDecoder`.

## When you need a different key name

Let's say for the JSON, you want the key name to be "number_of_legs" (snake cased), instead of "numberOfLegs".

To customize the JSON key names, add `CodingKeys` enum to the struct.

```swift
struct Animal: Codable {
    var numberOfLegs: Int
    enum CodingKeys: String, CodingKey {
        case numberOfLegs = "number_of_legs"
    }
}
```

Now, there is some magic performed by the compiler with the `CodingKeys` enum. The compiler only recognize the enum name "CodingKeys", reserved as the keys for the struct. 

_Note: `CodingKeys` is a compiler recognized enum, while `CodingKey` is the protocol._

You may in fact have your own type that implements the `CodingKey` protocol. Read on.

## When you need a nested structure

When your struct is flat, but maps to a nested structure in the JSON, you have more work to do.

```
{
    "anatomy" : {
        "numberOfLegs" : 2
    }
}
```

Firstly, you have to add your keys for the nested. We add `AnatomyCodingKeys` which implements `CodingKey` protocol.

```swift
struct Animal {
    var numberOfLegs: Int
    
    enum CodingKeys: String, CodingKey {
        case anatomy
    }
    
    enum AnatomyCodingKeys: String, CodingKey {
        case numberOfLegs
    }
}
```

Then you implement `Encodable` and `Decodable`.

```swift
extension Animal: Encodable {
    func encode(to encoder: Encoder) throws {
        // #1
        var container = encoder.container(keyedBy: CodingKeys.self)
        // #2 and #3
        var anatomyContainer = container.nestedContainer(keyedBy: AnatomyCodingKeys.self, forKey: .anatomy)
        // #4
        try anatomyContainer.encode(numberOfLegs, forKey: .numberOfLegs)
    }
}
```

The encoding process is as such:

1. Get the main container with keys as per `CodingKeys`
2. Get the nested container, which is in the main container at the key "anatomy" (of `CodingKeys`)
3. Main container has keys as per `AnatomyCodingKeys` (aka "keyed by")
4. Encode each type with key

The decoding is similar.

```swift
extension Animal: Decodable {
    init(from decoder: Decoder) throws {
        let container = try decoder.container(keyedBy: CodingKeys.self)
        let anatomyContainer = try container.nestedContainer(keyedBy: AnatomyCodingKeys.self, forKey: .anatomy)
        numberOfLegs = try anatomyContainer.decode(Int.self, forKey: .numberOfLegs)
    }
}
```

## Error with a Dictionary member

Let's look at an unexpected scenario, a struct having a Dictionary as it's member. 

```swift
struct Sword: Codable {
    var properties: Dictionary<String, Codable>
}
```

`Sword` has a flexible member `properties`, which basically can store any key-value pair. But there will be a compile error.

    Type 'Sword' does not conform to protocol 'Encodable'
    Type 'Sword' does not conform to protocol 'Decodable'

The problem is because a `Dictionary` is not a Codable, even thought the values in it is.

It such case, you will need dynamic coding keys, an advanced topic.

## Dynamic Coding Keys

[Apple codable playground](https://developer.apple.com/documentation/foundation/archives_and_serialization/using_json_with_custom_types) provides a sample code on how you can have a dynamic key eg. the keys are not defined exhausively in the `CodingKeys` enum.

In our scenario, that's what we want for the Dictionary, where the keys in it can be any string.

Create `DynamicKey`, which implements `CodingKey`, but it only can be init with a string.

```swift
struct DynamicKey: CodingKey {
    var stringValue: String
    init?(stringValue: String) {
        self.stringValue = stringValue
    }
    var intValue: Int? { return nil }
    init?(intValue: Int) { return nil }  
}
```

Then we extend `KeyedEncodingContainer` to provide the method to encode the dictionary.

```swift
extension KeyedEncodingContainer where Key == DynamicKey {    
    mutating func encodeDynamicKeyValues(withDictionary dictionary: [String : Any]) throws {
        for (key, value) in dictionary {
            let dynamicKey = DynamicKey(stringValue: key)!
            switch value {
            case let v as String: try encode(v, forKey: dynamicKey)
            case let v as Int: try encode(v, forKey: dynamicKey)
            default: print("Type \(type(of: value)) not supported")
            }
        }
    }
}
```

The above `encodeDynamicKeyValues` has a shortfall: you need to add to the types supported. The above code illustrated only for String and Int. _If you know of a better approach, let me know!_

To use, in `encode(to:)`,

```swift
var propertiesContainer = container.nestedContainer(keyedBy: DynamicKey.self, forKey: .properties)
if let properties = properties {
    try propertiesContainer.encodeDynamicKeyValues(withDictionary: properties)
}
```

I will leave the implementation of `KeyedDecodingContainer` as an exercise :)

Or check [my gist](https://gist.github.com/samwize/a82f29a1fb34091cd61fc06934568f82).

## What is a container?

If you need to customize the encoding and decoding, you will need to grasp the concept of containers.

[![](/images/wwdc-foundation-codable.png)](/2017/08/03/wwdc-2017-whats-new-in-foundation/)

A container is one of 3 types:

1. Keyed Container – provides values by keys, like a dictionary
2. Unkeyed Container – provides ordered values without keys, like an array
3. Single Value Container – a single raw value

In encoding/decoding, you need to use the correct type of container as per the JSON/whatever structure you have.

## Resources

Codable is [open source](https://github.com/apple/swift/blob/master/stdlib/public/core/Codable.swift), so we can dig it and understand how it works internally.

A more useful resource provided by Apple is the [playground for custom type](https://developer.apple.com/documentation/foundation/archives_and_serialization/using_json_with_custom_types), showing how we could achieve dynamic coding keys.

Apple has a [basic guide](https://developer.apple.com/documentation/foundation/archives_and_serialization/encoding_and_decoding_custom_types). Other good guides include [swiftjson.guide](http://swiftjson.guide) and [raywenderlich's](https://www.raywenderlich.com/172145/encoding-decoding-and-serialization-in-swift-4)
