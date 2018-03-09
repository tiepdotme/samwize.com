---
layout: post
title: "How to Make Xcode Run Faster"
date: 2018-02-28T15:34:12+08:00
categories: [Xcode]
---

## Show Build Time

You have to enable this settings via command line:

    defaults write com.apple.dt.Xcode ShowBuildOperationDuration -bool YES

Whenever Xcode finish building, you will see the time it spent in the top status panel (where you see build "Succeeded").

With this data displayed, let's make Xcode build faster.

## Whole Module Optimization

The default settings from Xcode makes the build process slow. You may say it is a bug.

Do this for your target's **Debug configuration**:

1. In Build Settings > Optimization Level > change to **Fast, Whole Module Optimization**
2. In Build Settings > Swift Flags > Add `-Onone`

If you are not sure, [screenshots here](http://developear.com/blog/2016/12/30/Speed-Swift-Compilation.html).

## Pods too

Similarly, you can add a post install hook to `Podfile`.

```ruby
post_install do |installer|
  installer.pods_project.targets.each do |target|
    target.build_configurations.each do |config|
      if config.name == 'Debug'
        config.build_settings['OTHER_SWIFT_FLAGS'] = ['$(inherited)', '-Onone']
        config.build_settings['SWIFT_OPTIMIZATION_LEVEL'] = '-Owholemodule'
      end
    end
  end
end
```

## Build Active Architecture Only

The default from Xcode is correct, but you should double check.

Build Settings > Build Active Architecture > should be **Yes for Debug configuration**.

What this means is that [Xcode will detect](http://samwize.com/2015/01/14/what-is-build-active-architecture-only/) the device that is connected and build only for that architecture alone.

## Other Tricks

If you have followed the recommendations above, you should see significant improvement in your build time.

If not, this [wiki on github](https://github.com/fastred/Optimizing-Swift-Build-Times#whole-module-optimization) provides other tricks and good settings. 
