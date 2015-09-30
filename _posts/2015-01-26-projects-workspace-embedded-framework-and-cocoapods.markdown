---
layout: post
title: "Projects, Workspace, Embedded Framework and CocoaPods"
date: 2015-01-26 09:00:23 +0800
comments: true
categories: [iOS]
---

There are a few ways to include a common library in an Xcode project.

For iOS 8, it became interesting when embedded/dynamic framework is made available for iOS. You can watch about "modern framework" in [WWDC 2014](https://developer.apple.com/videos/wwdc/2014/?id=416).

This post is to clarify some of the concepts.

<!-- more -->

# Embedded/Dynamic Framework

Prior to iOS 8, developers can also create their own framework as [static framework](http://www.raywenderlich.com/65964/create-a-framework-for-ios).

For iOS 8, dynamic framework is introduced because it would be needed for iOS Extension and Watch app. 

For example, you would have common code shared between the parent app and the Watch app (note: they are separately sandboxed). It is aptly to put these common code in a framework.

If you were to be using static framework, then both the parent app and Watch will have to contain a copy of the static framework. There is redundancy.

With dynamic framework, you will only need to have 1 copy and both the parent app and Watch app can use it.

But this really should be called [embedded framework](http://stackoverflow.com/questions/25080914/will-ios-8-support-dynamic-linking/26403330#26403330), because the library can only be used by your own apps; that is your parent app, extensions, or watch apps.

It cannot be used by other third party apps. Therefore it is [not truly dynamic](https://developer.apple.com/library/mac/documentation/MacOSX/Conceptual/BPFrameworks/Concepts/FrameworkBinding.html).


# Projects

We all starts a project with a _project_. 

A project can have multiple targets and frameworks. 

Yes, you create an embedded framework in your project.


# Workspace

Then there is the concept of _workspace_ in Xcode.

Workspace is for handling multiple projects which are inter-related.


# CocoaPods

If you have been using CocoaPods, you would be opening _workspace_ instead of _project_ because CocoaPods created workspace which contain 2 projects - your app and the pods project.

However, CocoaPods is currently [not playing well with embedded framework](https://github.com/CocoaPods/CocoaPods/pull/2222).

This is because your watch app has to include another copy of the pods, instead of sharing with the parent app. 

Conceptually, this can be solved by having the pods as an embedded framework. We have to wait for the CocoaPods maintainers to support the feature.


# If not using CocoaPods..

Since CocoaPods is not playing well with embedded framework at the moment, this is what you can do if you want to using your common framework across multiple projects..

Create your embedded framework as a project of it's own, and `git` it. 

Create your main application as a project of it's own. Then create a workspace and include this project.

Using `git submodule`, add the embedded framwork project into the workspace directory. Then add the project into the workspace.

With that, the workspace is setup with the 2 projects.

