---
layout: post
title: "Integrating Static Library With Cocoapods Use_frameworks"
date: 2016-05-05T09:33:10+08:00
categories: [iOS]
---

When using a library that is built with static framework (vs dynamic) and integrating using [Cocoapods](https://blog.cocoapods.org/CocoaPods-0.36/), you could have linker error when you use `use_frameworks!` and Swift.

There is a workaround.

Let's take the example of `Some-iOS-SDK` that uses static framework.

You can use post_hook in your `Podfile` as such:

```ruby
use_frameworks!

post_install do |installer|
    installer.pods_project.targets.each do |target|
        target.build_configurations.each do |config|
            if target.name == "Some-iOS-SDK"
                config.build_settings["OTHER_LDFLAGS"] = '$(inherited) "-ObjC"'
            end
        end
    end
end
```

iOS, and [CocoaPods](https://github.com/CocoaPods/CocoaPods/issues/3701), is moving away from static frameworks (towards dynamic). So until libraries turn their project to using dynamic framework, the workaround is needed.
