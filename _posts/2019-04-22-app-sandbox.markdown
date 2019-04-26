---
layout: post
title: "App Sandbox"
date: 2019-04-22T16:57:03+08:00
published: false
categories: [macOS]
---

[App Sandbox](https://developer.apple.com/library/archive/documentation/Security/Conceptual/AppSandboxDesignGuide/AboutAppSandbox/AboutAppSandbox.html) control the access that an app has in macOS.

It is a capability that you can enable/disable in entitlements.

When App Sandbox is enabled, the `FileManager` will behave differently. If you try to get the user's Document directory, it is NOT `~/Documents`. Instead, it is in the app's container eg. `~/...`

https://developer.apple.com/library/archive/documentation/FileManagement/Conceptual/FileSystemProgrammingGuide/FileSystemOverview/FileSystemOverview.html

## NSOpenPanel

Use `NSOpenPanel` to provide the system UI to select a file/folder _outside_ of the app's container.

Once a user provide a folder to use, you can bookmark and use it subsequently.

https://developer.apple.com/library/archive/documentation/FileManagement/Conceptual/FileSystemProgrammingGuide/UsingtheOpenandSavePanels/UsingtheOpenandSavePanels.html
