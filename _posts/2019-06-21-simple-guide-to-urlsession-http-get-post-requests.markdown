---
layout: post
title: "Using URLSession for simple HTTP GET & POST"
date: 2019-06-21T18:25:00+08:00
categories: [Foundation]
---

Since `URLSession` was introduced in iOS 7, [third party libraries](/2014/05/10/tutorial-on-using-afnetworking-2-dot-0/) have somehow lesser value.

This post is a quick guide with snippets to some common use cases when working with HTTP APIs using the first party Foundation framework.

## HTTP GET

```swift
let url = URL(string: "https://samwize.com/api/hero")!
let task = URLSession.shared.dataTask(with: url) { data, response, error in
    print(String(data: data!, encoding: .utf8))
}
task.resume()
```

Just that few lines of code to fetch from a URL.

The `dataTask()` method is a HTTP GET request.

## HTTP POST

```swift
let url = URL(string: "https://samwize.com/api/hero")!
var request = URLRequest(url: url)
request.httpMethod = "POST"
let payload = "id=1234".data(using: .utf8)!
let task = URLSession.shared.uploadTask(with: request, from: payload) { data, response, error in
    print(String(data: data!, encoding: .utf8))
}
task.resume()
```

The `uploadTask()` method is for a POST request. Or you may change the `httpMethod` to UPDATE etc.

## POST with query string

The POST request above is a simple example, with a payload that doesn't require any percent encoding. But it is more likely that you have longer query items.

`URLQueryItem` (also Foundation framework) provides a basic struct for constructing key-value pairs.

Together with `URLComponents`, you can add as many key-value pairs to a `URLComponents`, then access the `percentEncodedQuery`.

```swift
var urlComponents = URLComponents()
let q1 = URLQueryItem(name: "name", value: "with space that needs encoding")
urlComponents.queryItems = [q1]
let payload = urlComponents.percentEncodedQuery!.data(using: .utf8)
```

## Codable Model

New in Swift 4, JSON can be decoded to a `struct` easily,

I have a separate guide to [Codable struct](/2017/09/26/guide-to-using-codable-struct-for-json-the-new-thing-in-swift-4/).

Let's say in the POST API, a `Hero` object in JSON is returned, we could decode it easily.

```swift
let task = URLSession.shared.uploadTask(with: request, from: payload) { data, response, error in
    let decoder = JSONDecoder()
    let hero = try! decoder.decode(Hero.self, from: data!)
}
```

Similarly, you can encode a model, then POST the JSON string/data as a payload.

## Other Resources

Apple's official guide on [URL Loading System](https://developer.apple.com/documentation/foundation/url_loading_system) and [how to fetch data](https://developer.apple.com/documentation/foundation/url_loading_system/fetching_website_data_into_memory) is their guide to the networking framework. You could use `URLSessionConfiguration` to configure your own session, and also use `URLSessionDelegate` for a delegate pattern to receive the data.
