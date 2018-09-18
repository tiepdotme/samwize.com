---
layout: post
title: "UserDefaults"
date: 2018-09-14T17:20:09+08:00
categories: [Foundation]
published: false
---

`UserDefaults` is the simplest way to persist key-values in iOS and macOS. Yet there is more to know about it.

## How it works

There are 4 ways to set the key-value, and there are differences and use cases for each.

![UserDefaults - How it works](/images/userdefaults.jpg)

## Observe with KeyPath

```swift
extension UserDefaults {
  @objc dynamic var showAtLaunch: Bool {
    return self.value(forKey:"showAtLaunch") as? Bool ?? false }
  }
  let observation = UserDefaults.standard.observe(\.showAtLaunch) { observed, change in
    preferencesChanged()
  }
```
