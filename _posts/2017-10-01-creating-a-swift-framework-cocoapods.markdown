---
layout: post
title: "Steps to Create Swift Framework + Cocoapods"
date: 2017-10-01T15:32:49+08:00
categories: [Swift]
---

## 1. Create framework project

Select Xcode > File > New > Project > iOS > **Cocoa Touch Framework**

![Cocoa Touch Framework](/images/xcode-template-ios-cocoa-touch-framework.jpg)

A Cocoa Touch Framework is for iOS, and can use UIKit framework. If you are developing a framework that does not require UIKit, you may select iMac > **Cocoa Framework**.

## 2. Add your source

As a simple example, add `MyFramework.swift`. I tend to keep my source file under `/Source`, but it's to your own preference.

```swift
public class MyFramework {
  static let shared = MyFramework()
  func foo() {}
}
```

This provide the client/app `MyFramework.shared.foo()`.

Notice that the class is **public**, and the members are implicitly public too. 

Read about [access level](/2017/04/20/access-levels-in-swift/), if needed. In short, to expose your classes and methods, it need to be public or open.

## 3. Create Cocoapods Spec

In your framework root directory, run

    pod spec create MyFramework
    
This will create a default `MyFramework.podspec`.

You will need to edit the podspec file to your needs. My bare minimum:

```ruby
Pod::Spec.new do |s|
  s.name         = "MyFramework"
  s.version      = "0.0.1"
  s.summary      = "A short description of MyFramework."
  s.description  = <<-DESC
  A much much longer description of MyFramework.
                   DESC
  s.homepage     = "http://EXAMPLE/MyFramework"
  s.license      = "Copyleft"
  s.author       = { "Junda" => "junda@just2us.com" }
  s.source       = { :path => '.' }
  # s.source       = { :git => "https://github/samwize/MyFramework", :tag => "#{s.version}" }
  s.source_files  = "Source/**/*.swift"
end
```

- `s.name` is the pod name, that will subsequently be used in Podfile
- `s.source` is using path to its current directory. This is temporary and you should uncomment and edit the source that points to the repository URL, when you have pushed to git.
- `s.source_files` is edited to include those  in `/Source` directory and only for Swift files.

Run `pod spec lint` to check if the podspec is ok. There will be 1 error on "Unsupported download strategy", but we can ignore that for now.

## 4. Add pod to project

Open the project that will be using the pod.

Add to `Podfile`

```ruby
pod 'MyFramework', :path => '../MyFramework'
# pod 'MyFramework', :git => 'https://github.com/samwize/MyFramework', :branch => 'master'
```

We use the local path, assuming MyFramework and the project are in the same folder.

Once again, uncomment the 2nd line that points to the git repository when you have pushed it.

With that, `pod install` and start developing with the pod!

_Note: The benefit of a local path is that you can make edit a Swift file in MyFramework, and it will be reflected right away in the project -- very convenient._