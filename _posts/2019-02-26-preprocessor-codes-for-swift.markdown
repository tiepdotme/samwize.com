---
layout: post
title: "Preprocessor Codes for Swift"
date: 2019-02-26T16:29:56+08:00
categories: []
---

I wrote about [using Preprocessor Codes in 2014](/2014/05/22/create-multiple-targets-slash-apps-for-1-xcode-project/), and also all the way back in [2009](https://blog.just2us.com/2009/07/tutorial-creating-multiple-targets-for-xcode-iphone-projects/).

This is an update to using it in Swift (vs Objective-C days).

## DEBUG flag

It is very common scenario where you want certain code to run only for the debug mode. For example [Admob has a tool](https://developers.google.com/admob/ios/mediation-test-suite) for debugging their SDK integrations.

You want code like this:

```swift
#if DEBUG
import GoogleMobileAdsMediationTestSuite
#endif

func foo() {
    #if DEBUG
    GoogleMobileAdsMediationTestSuite.present(withAppID: "abc", on: self, delegate: nil)
    #endif
}
```

## Build Settings

The `DEBUG` flag is not automatically provided. Do not confuse with the debug build configuration.

But we do want the debug build configuration to have the debug flag.

1. Go to **Project > Build Settings > Other Swift Flags**
2. Add `-DDEBUG` for the Debug build configuration
3. Do NOT add for the Release build configuration

If your flag name is BANANA, then add `-DBANANA`.

The biggest difference is that we now add to swift flags.

For Objective-C, we add to **Preprocessor Macros**. So if you do have Objective-C codes and want to use the same flag, then you must add to Preprocessor Macros too.

## What about cocoapods library?

The Swift flag for a project or target will not apply to pods.

You have to set it manually. But since every `pod install` will reset your changes, you should add a post hook to `Podfile` like this:

```ruby
post_install do |installer|
    installer.pods_project.targets.each do |target|
        if target.name == 'Armchair'
            target.build_configurations.each do |config|
                if config.name == 'Debug'
                    config.build_settings['OTHER_SWIFT_FLAGS'] = '-DDebug'
                    else
                    config.build_settings['OTHER_SWIFT_FLAGS'] = ''
                end
            end
        end
    end
end
```

You could add for every pods. In the config above, we add only for the pod [Armchair](https://github.com/UrbanApps/Armchair).
