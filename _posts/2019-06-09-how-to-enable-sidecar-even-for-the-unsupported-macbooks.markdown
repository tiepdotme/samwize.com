---
layout: post
title: "How to Enable Sidecar for the Unsupported Macbooks"
date: 2019-06-09T15:09:30+08:00
categories: [Mac]
---

[macOS 10.15 Catalina](https://www.macrumors.com/2019/06/06/macos-catalina-sidecar-limited-to-newer-macs/) (still beta) now supports running a secondary display using an iPad!

I was disappointed to realize that it does not support for my old **MacBook early 2015**. I was running an iPad Mini (5th generation 2019), but I know the problem should be due to the MacBook which is driving the display.

But I got it working!

![My 2015 MacBook + iPad Mini](/images/sidecar-with-macbook-2015.jpg)

## Solution

It turned out that Sidecar is being hidden for some older hardware, and with some simple overriding of the defaults, we can make it show.

Open your iTerm/Terminal and give these commands:

```bash
defaults write com.apple.sidecar.display hasShownPref -bool YES
defaults write com.apple.sidecar.display allowAllDevices -bool YES
open /System/Library/PreferencePanes/Sidecar.prefPane
```

## Error on my first attempt

![Error](/images/sidecar-pref-error.jpg)

_On my first attempt, it throws me that error. But perhaps due to a reboot, the Sidecar shows up mysteriously the next day. So don't panic if it didn't work at first._
