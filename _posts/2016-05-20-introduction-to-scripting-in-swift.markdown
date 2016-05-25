---
layout: post
title: "Introduction to Scripting in Swift"
date: 2016-05-20T11:16:26+08:00
categories: [Swift]
---

## Getting Started

Assuming you already have Swift installed in your system (that is, if you run `whereis swift`, you should see `/usr/bin/swift`), running scripts in Swift is pretty easy.

The process of writing Swift script is similar to that of bash script or python script.

Create/touch `main.swift` (it must be named main), and write the following hello code:

```swift
#!/usr/bin/swift
print("Hello Swift!")
```

The first line is the hashbang commonly seen in other scripting languages, which points to our Swift REPL.

To run:

    chmod +x main.swift
    ./main.swift

This will print "Hello Swift!".



## Compiling to Binary

In Getting Started, we have a swift file that compile on the fly.

We could compile that into an **executable binary**.

    swiftc main.swift -o hello

We name the binary `hello` in above. The binary will be created, and now you can run it with `./hello`.



## Swift version manager

If you have used rvm for ruby, we have the equivalent [chswift](https://github.com/neonichu/chswift), by @neonichu from Cocoapods.

Taking a step further, @neonichu created [cato](https://github.com/neonichu/cato), which let you run script with hashbang directive that specify the Swift version to run with:

    #!/usr/bin/env cato 1.2

There is also [swiftenv](https://github.com/kylef/swiftenv), another Swift Version Manager, which is more popular likely because it has much similarities to [pyenv](https://github.com/yyuu/pyenv).


## Dependency Manager

Cocoapods has a [rome version](https://github.com/neonichu/Rome) that will build frameworks for Swift scripts to use.

Install with:

    gem install cocoapods-rome

Till further, that's all for an introduction. 
