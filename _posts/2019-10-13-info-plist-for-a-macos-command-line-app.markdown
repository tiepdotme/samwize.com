---
layout: post
title: "Info.plist for a macOS Command Line App"
date: 2019-10-13T01:29:12+08:00
categories: [macOS]
---

After upgrading to Catalina, one of my [favorite tool](/2016/02/22/have-fun-with-git-lol-commits/) broke.

> This app has crashed because it attempted to access privacy-sensitive data without a usage description.  The app's Info.plist must contain an NSCameraUsageDescription key with a string value explaining to the user how the app uses this data.

Seems like Catalina started to enforce on apps to declare the reason for using camera/etc.

In the world of iOS, this is common, and we simply add to Info.plist.

But for command line tool (CLI), they are simply a binary, and has no Info.plist. So then how to add an Info.plist?

## Solution

Turns out the solution is simply **have the Info.plist file be in the same directory as the binary**.

In my [fix](https://github.com/samwize/imagesnap/commit/569d19af64448124aae156b1786ec08d3739343e) to imagesnap lib, I have not just added Info.plist, but also to copy the file during Build Phase.

1. Go to Build Phases > Copy Files
2. Destination set to Products Directory
3. Add Info.plist
4. Uncheck "Copy only when installing"

This setup would be very necessary for CLI kind of app. I'm surprised that Xcode didn't automate the setup.

On any case, lolcommits is [fixed](https://github.com/samwize/lolcommits/commit/306dd0ebf21464de2d7824cfae841517cd094b1f) and I can take more photo of me fixing stuff 🤠
