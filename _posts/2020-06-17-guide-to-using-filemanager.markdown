---
layout: post
title: "Guide to Using FileManager"
date: 2020-06-17T15:18:24+08:00
categories: [Foundation]
published: false
---

This Swift API is one of the most confusing to use. Not so long ago I discovered that [inits for file and string](/2020/02/09/pitfall-url-for-local-files/) can result in different behaviours.

This time, I find that this API still has very un-swifty ways of doing things.

## Is this a directory?

This very common operation to know if a URL is a file or a directory is not as easy as you think. In other languages (even very old ones), there is usually a `isDirectory()`.

But the Swift answer is:

```swift
var isDir: ObjCBool = false
guard fileManager.fileExists(atPath: someURL.path, isDirectory: &isDir) else { return }
if isDir.boolValue {
    // This is a directory
}
```

Yes, it is the only way, using `UnsafeMutablePointer<ObjCBool>`.

I use a convenient extension that works on URL or path string, and try to forget the above un-swifty way..

```swift
public extension FileManager {
    func isDirectory(at url: URL) -> Bool {
        isDirectory(atPath: url.path)
    }

    func isDirectory(atPath: String) -> Bool {
        var isDirectory: ObjCBool = false
        let exists = fileExists(atPath: atPath, isDirectory: &isDirectory)
        return exists && isDirectory.boolValue
    }
}
```

_You might have notice, FileManager uses `at` for URL and `atPath` for String paths. I'll just stick to their convention._

## Rest of the guide

...
