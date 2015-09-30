---
layout: post
title: "Auto Increment Build Number in Xcode"
date: 2014-06-15 16:35:05 +0800
comments: true
categories: iOS
---

You can auto increase the number with this [build script](http://stackoverflow.com/q/10091310/242682).

However, that stackoverflow thread has closed and the 'solution' will not work now.

<!-- more -->

## The Solution

This is the working script that increment the integer instead of hex.

Add it to your build phase's run script:

    #!/bin/bash
    bN=$(/usr/libexec/PlistBuddy -c "Print CFBundleVersion" "$INFOPLIST_FILE")
    bN=$(expr $bN + 1)
    bN=$(printf "%d" $bN)
    /usr/libexec/PlistBuddy -c "Set :CFBundleVersion $bN" "$INFOPLIST_FILE"

I will usually set `CFBundleVersion` (build number) to `1000` for a new project to begin with.

The corresponding `CFBundleShortVersionString` is any string you want to name your version eg. "1.0" or "0.0.9-alpha"


## Improved to use common number across targets

I also like the [idea](http://stackoverflow.com/q/9258344/242682) of having a common build number across different targets, and keeping that number in a separate file. But that script wasn't provided..

And so I adapted the above solution by adding a `Info-CFBundleVersion.plist` that contains the common number.

    #!/bin/bash
    bN=$(/usr/libexec/PlistBuddy -c "Print CFBundleVersion" "$PROJECT_DIR/Resources/Info-CFBundleVersion.plist")
    bN=$(expr $bN + 1)
    bN=$(printf "%d" $bN)
    /usr/libexec/PlistBuddy -c "Set :CFBundleVersion $bN" "$INFOPLIST_FILE"
    /usr/libexec/PlistBuddy -c "Set :CFBundleVersion $bN" "$PROJECT_DIR/Resources/Info-CFBundleVersion.plist"

