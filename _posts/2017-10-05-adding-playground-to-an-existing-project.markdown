---
layout: post
title: "Adding Playground to an Existing Project"
date: 2017-10-05T16:26:25+08:00
categories: [Playground]
---

Playground is a very useful tool, especially for building UI, as you can preview them almost right away, saving many hours building and running on simulator.

New in Xcode 9, we can now [add custom framework to Playground](http://help.apple.com/xcode/mac/9.0/#/devc9b33111c), therefore providing a quick way to integrate with existing project.

This post will show how to add Playground to an existing project, using Cocoapods along in it, and interacting with UI controls for instant feedback.

![](/images/xcode-playground-ui-live.jpg)

## 1. Open your project workspace

I am assuming you are already using workspace.

But if you are not, open the .xcodeproj and File > Save As Workspace.

## 2. Add new target

Select **File > New > Target > Cocoa Touch Framework**.

![](/images/xcode-template-ios-cocoa-touch-framework.jpg)

Let's name the target "MyUIFramework".

This target builds the framework, which provides code available to use in Playground.

## 3. Add files to target

Add a new Swift file, making sure it is available to the target.

In File Inspector, make sure the target "MyUIFramework" is checked.

You can also add existing files to the framework, exposing them to Playground later.

## 4. Build the framework

Select the Scheme and Build. 

You must manually build each time you made changes to the framework.

## 5. Add Playground

In the workspace, **File > New > Playground > Single View**.

In the Playground page, we can import the framwork, and set up a live view.

```swift
import PlaygroundSupport
import UIKit
import MyUIFramework

class MyViewController : UIViewController {
    override func loadView() {
        let view = UIView()
        view.backgroundColor = .red
        // Add subviews ...
        self.view = view
    }
}

PlaygroundPage.current.liveView = MyViewController()
```

You create your views in `loadView`, and see them in Live View (select Show Assistant Editor).

Just like Playground, code changes will be reflected _almost_ right away.

## 6. Using Cocoapods 

You cannot import a Cocoapods module in playground, yet.

If the Swift file requires the use of Cocoapods, you have to make it available to the target as specified in Podfile.

We make [Cartography](https://github.com/robb/Cartography), an awesome library for constructing autolayout, for the framework:

```ruby
target 'MyUIFramework' do
    pod 'Cartography', '~> 1.1.0'
end
```

`pod install`, build framework again, and you will now be able to `import Cartography` in Playground too.

Because now 2 targets are using the same Cargography lib, it is cleaner to [define shared pods](https://www.natashatherobot.com/cocoapods-installing-same-pod-multiple-targets/) in Podfile. We leave that as an exercise for you.