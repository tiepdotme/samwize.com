---
layout: post
title: "Everything About UINavigationBar"
date: 2016-02-24T16:32:56+08:00
categories: [iOS]
published: false
---

In all examples, we will often refer to `vc1` and `vc2`. The initial view controller shown is `vc1`, and it will push to the next view controller `vc2`.


## Remove "Back" text (only chevron)

```swift
navigationItem.backBarButtonItem = UIBarButtonItem(title: "", style: .Plain, target: nil, action: nil)
```

This should be called in `vc1`.


## Status Bar

This is not precisely about navigation bar, but status bar.

To style status bar, you will need to set navigation bar to a corresponding style (the opposite).

Eg. status bar Light, navigation bar must be Black

```swift
// This don't make nav bar Black, but instead, make sure status bar is Light
navigationController?.navigationBar.barStyle = .Black
```

