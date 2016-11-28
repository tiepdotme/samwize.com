---
layout: post
title: "Migrating an iOS Project From Objective-C to Swift"
date: 2016-11-22T23:09:55+08:00
categories: []
---

# UIApplicationDelegate

Start with the app delegate.

Add a `AppDelegate.swift`, and mark the class with `@UIApplicationMain` attribute.

Remove `main.m`, because it is now unnecessary. With the use of `@UIApplicationMain`, the target now knows the entry point.
