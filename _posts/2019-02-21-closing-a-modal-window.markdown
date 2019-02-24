---
layout: post
title: "Closing a Modal Window"
date: 2019-02-21T14:50:12+08:00
categories: [macOS]
---

Window(s) is a more important concept for macOS than in iOS, because a mac app can have multiple windows while iOS has only 1 key window.

Window has certain functionalities, many [styles](https://github.com/lukakerr/NSWindowStyles), but it is cumbersome to create. So we are used to creating `NSViewController` as a scene.

If you were to create a modal window conveniently, eg. using `presentingViewController.presentAsModalWindow(anotherViewController)`, you will need some work to handle window events.

This code handles the **window close event**, which can happen in 2 ways:

1. User press ESC
2. User clicks on the red close button

```swift
class AnotherViewController: NSViewController {

    override func viewDidLoad() {
        super.viewDidLoad()
        view.window?.delegate = self
    }

    // Press ESC
    override func cancelOperation(_ sender: Any?) {
        do {
            try save()
            dismiss(sender)
        } catch {
            presentError(error)
        }
    }

    private func save() throws {
        // Validate and throw errors
    }

}

extension AnotherViewController: NSWindowDelegate {

    // Click window close button
    func windowShouldClose(_ sender: NSWindow) -> Bool {
        do {
            try save()
            return true
        } catch {
            presentError(error)
            return false
        }
    }

}
```

The code is pretty self explanatory.

The gist is that Cocoa `NSViewController` becomes the window's delegate.

`NSWindowDelegate` has more events, such as `windowWillClose`, `windowWillMiniaturize`, `customWindowsToEnterFullScreen` etc, that you can make use of.

`cancelOperation` is the keyboard shortcut of pressing ESC. You have to call `dismiss` explicitly, which will NOT ask for `windowShouldClose`.
