---
layout: post
title: "Advanced Swift Scripting"
date: 2016-06-15T05:02:09-07:00
categories: [Swift]
---

This post covers more common tasks when writing Swift scripts, following the post on [an introduction to Swift scripting](/2016/05/20/introduction-to-scripting-in-swift/).

## Shell

You can [invoke shell](http://stackoverflow.com/a/26972043/242682) to execute:

```swift
func shell(launchPath: String, arguments: [String]) -> String {
    let task = NSTask()
    task.launchPath = launchPath
    task.arguments = arguments
    
    let pipe = NSPipe()
    task.standardOutput = pipe
    task.launch()
    
    let data = pipe.fileHandleForReading.readDataToEndOfFile()
    let output: String = NSString(data: data, encoding: NSUTF8StringEncoding)! as String
    
    return output
}
```

To use:

```swift
shell("/bin/pwd", arguments: [])
shell("/bin/cp", arguments: ["/path/to/file", "/path/to/another/file"])
```

## Launch Arguments

Pass in [launch arguments](http://ericasadun.com/2014/06/12/swift-at-the-command-line/) to your script with `NSProcessInfo`. 

```swift
let arguments = NSProcessInfo.processInfo().arguments as [String]
let dashedArguments = arguments.filter({$0.hasPrefix("-")})

for argument : NSString in dashedArguments {
    let key = argument.substringFromIndex(1)
    let value : AnyObject? = NSUserDefaults.standardUserDefaults().valueForKey(key)
    print("\(key)=\(value)")
}
```

_Note that the value is retrievable via `NSUserDefaults`_

Example: `./main.swift  -s 123 --mode godlike --nokill`

Keys: `s`, `-mode` and `-nokill`
Values: `123` (integer), `godlike` (string) and nil 

