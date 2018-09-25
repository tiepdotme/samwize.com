---
layout: post
title: "App Group for Sharing Between Apps and Extensions"
date: 2018-09-23T11:10:54+08:00
categories: [Foundation]
---

**App Group** is a feature that is seldom mentioned, until you try to share data between apps, or to [extensions](https://developer.apple.com/library/archive/documentation/General/Conceptual/ExtensibilityPG/ExtensionScenarios.html).

Another type of group for sharing is **Keychain Access Group**, specially for keychains.

## 3 Types of Access Group

1. Private Group
  - Automatically created for each app
  - Only 1
2. Application Group
  - Optional
  - Multiple groups
  - 1 app can be in multiple groups
3. Keychain Access Group
  - Optional
  - Multiple groups
  - 1 keychain item can be in only 1 group

## 1. Private Group

We don't often talk about private group, because it simply the default, automatically created, aka the sandbox for an app.

Every app has a unique App ID, each representing a private group. Let's get the IDs right with examples:

**Bundle ID** = "com.just2us.app1", "com.just2us.app2", etc

**Team ID**, [aka App Prefix ID](/2018/09/20/pitfall-on-app-id-prefixes-legacy/) = "ABCDE12345"

**App ID** = Team ID + Bundle ID  = "ABCDE12345.com.just2us.app1", "ABCDE12345.com.just2us.app2", etc

App IDs are unique as you can see, and they represent the private group (container).

By default, all frameworks such as `UserDefaults`, `Keychain`, `FileManager` use the private group.

## 2. Application Group

Application groups are like private group, but custom. You can create them by enabling the capability for your app. You can create multiple groups, and use in multiple apps.

**App Group ID** = "group.com.just2us.app1", "com.just2us.family"

An app group is like an app Bundle ID, usually the convention is to prefix a "group" to the app bundle ID, thus sharing between a host app with it's extensions. But you can use any string, like I use "com.just2us.family" which share among ALL apps.

When an app have access to an app group, the app can access the container. For example:

```swift
let url = FileManager.default.containerURL(forSecurityApplicationGroupIdentifier: "com.just2us.family")
let userDefaults = UserDefaults(suiteName: "com.just2us.family")
```

## 3. Keychain Access Groups

When you want multiple apps to share a keychain, then you need to create a common Keychain Access Group. Again, enable the feature under capability.

**Keychain Access Group ID** = "ABCDE12345.com.just2us.family", "ABCDE12345.com.just2us.another", etc

If you did NOT create any custom keychain access group, by default, you still have one using the App ID eg. "ABCDE12345.com.just2us.app1".

Note that keychain access group ID has the Team/App Prefix ID. In an app, you have only 1 Team/App Prefix ID. This is important to know because a keychain is tied to this Team/App Prefix ID. If you change the prefix ID ([legacy reason](/2018/09/20/pitfall-on-app-id-prefixes-legacy/)), then you will lose data in the previous keychain.

Keychain contain items, and each item must be in exactly 1 Keychain Access Group.

So when you create keychain item, you must specify the group it is in, via the `kSecAttrAccessGroup` key.

```swift
var query: [String: Any] = [kSecAttrAccessGroup as String: "ABCDE12345.com.just2us.family", ...]
```
