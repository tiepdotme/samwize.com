---
layout: post
title: "Resolving CocoaPods build error due to targets building for only active architecture"
date: 2014-05-15 11:01:26 +0800
comments: true
categories: [iOS]
---

I always have a build error after `pod install`, because by default, CocoaPods will configure your pods to **build for active architecture only** (during debugging).

The solution is to change Pods project setting `Build Active Architecture Only` to `NO` for debug.

But it is tedious to change every time after you run `pod install` (as your change will get reverted back to `YES`).

<!-- more -->

As [suggested by msmollin](https://github.com/CocoaPods/CocoaPods/issues/1400#issuecomment-39855490) in CocoaPods issues, you can fix by with:

```ruby
# Append to your Podfile
post_install do |installer_representation|
    installer_representation.pods_project.targets.each do |target|
        target.build_configurations.each do |config|
            config.build_settings['ONLY_ACTIVE_ARCH'] = 'NO'
        end
    end
end
```
