---
layout: post
title: "Bumping Build Number"
date: 2015-01-30 09:14:03 +0800
comments: true
categories: iOS
---

Every new version uploaded to iTunes Connect needs to have a different build number in order to differentiate the version. 

Note: Build number (eg. 1234) is different from Version number (eg. 1.2).

The build number can be automatically increased in these few ways:

<!-- more -->

# Bump in a build script

Create a **Run Script** in your project **Build Phases**

    #!/bin/bash
    bN=$(/usr/libexec/PlistBuddy -c "Print CFBundleVersion" "$INFOPLIST_FILE")
    bN=$(expr $bN + 1)
    bN=$(printf "%d" $bN)
    /usr/libexec/PlistBuddy -c "Set :CFBundleVersion $bN" "$INFOPLIST_FILE"

This will auto increment the build number every time you build the project.

# Make use of git commits

If you work in a team, the previous build script will pose an unsyncronized problem.

As an [improvement](https://objcsharp.wordpress.com/2013/10/01/how-to-automatically-update-xcode-build-numbers-from-git/), you can use the total number of commits in your git repository as your build number!

    #!/bin/bash
    if [ ${CONFIGURATION} == "Release" ]; then
    buildNumber=$(git rev-list HEAD | wc -l | tr -d ' ')
    /usr/libexec/PlistBuddy -c "Set :CFBundleVersion $buildNumber" "${PROJECT_DIR}/${INFOPLIST_FILE}"
    fi;

# avgtool

Apple provide a tool to manage the build and version number.

Firstly, [setup the project](https://developer.apple.com/library/ios/qa/qa1827/_index.html) in order to use avgtool.

This is how you use it:

    # In your project folder
    # Let's check the current build number
    agvtool what-version

    # To auto increment the build number
    agvtool next-version -all

    # To set build number to a specify number eg. 1234
    agvtool new-version -all 1234

    # You can also set VERSION NUMBER eg 1.2
    agvtool new-marketing-version 1.2


# Using fastlane

[fastlane](/2015/01/29/fastlane-replacing-ui-screen-shooter-and-screenshot-uploader/) makes use of `agvtool` to bump the build number.

In any lane of Fastfile, you can add `increment_build_number`, which will basically call `agvtool next-version -all`

