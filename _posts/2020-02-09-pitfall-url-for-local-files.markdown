---
layout: post
title: "Pitfall: URL for Local Files"
date: 2020-02-09T21:02:00+08:00
categories: [iOS]
---

`URL` is a very common data structure, and can be used for http, file, and other schema.

But I often fell into the pitfalls of using it.

The biggest of all is using the wrong initializer.

## Initializations `URL(fileURLWithPath:)` vs `URL(string:)`

For **local files**, always use `URL(fileURLWithPath:)`.

The initialization does 2 things:

1. Encode if needed
2. Append `file://` if needed

So for the string path `/Document/Space .txt`, the initialization will create the URL `file:///Document/Space%20.txt`.

## Use `FileManager`

Use this for forming URL for local files:

```swift
let documentDirectory = try! FileManager.default.url(for: .documentDirectory, in: .userDomainMask, appropriateFor: nil, create: true)
let fileURL = documentDirectory.appendingPathComponent("secret.txt")
```

## Pitfall: `UIActivityViewController`

The `activityItems` can accept a `URL` to a local file, but they need to be in the format of `file://...`.

If it is not, it will not be sharable 🤔

So do this, if needed:

```
let fileUrl = URL(fileURLWithPath: url.absoluteString)
```

_Or just don't ever use `URL(string:)` for local files._
