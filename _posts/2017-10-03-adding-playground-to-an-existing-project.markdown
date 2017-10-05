---
layout: post
title: "Adding Playground to an Existing Project"
date: 2017-10-03T16:26:25+08:00
categories: [Playground]
---

Playground is a very useful tool, especially for building your UI, as you can preview them right away, saving your maybe an hour a day (assuming you wait an hour to build and run on simulator, which is not exaggerated).

In Xcode 9, we can now [add custom framework to Playground](http://help.apple.com/xcode/mac/9.0/#/devc9b33111c), therefore providing a quick way to integrate with existing project.

This post will show how to add Playground to an existing project, using Cocoapods along in it, and interacting with UI controls for instant feedback.

## 1. Open your project workspace

I am assuming you are already using workspace.

But if you are not, open the .xcodeproj and File > Save As Workspace.


## 2. Add new target

Select **File > New > Target > Cocoa Touch Framework**.

![](/images/xcode-template-ios-cocoa-touch-framework.jpg)

Let's name the target "MyUIFramework".

## 3. Add files to target

Add a Swift file, making sure it is available to the target.

## 4. In Playground

```swift
import MyUIFramework
```

## 5. Using Cocoapods 

If the Swift file requires the use of Cocoapods, you have to make it available to the target as specified in Podfile.


```ruby
target 'MyUIFramework' do
    pod 'Cartography', '~> 1.0.1'
end
```
