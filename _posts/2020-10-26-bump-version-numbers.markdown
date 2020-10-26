---
layout: post
title: "Bump Version Numbers"
date: 2020-10-26T17:29:27+08:00
categories: [fastlane]
---

This is a tip originally from [Ben Scheirman](https://benscheirman.com/2020/10/managing-version-numbers-with-fastlane/) on using Fastlane to bump the _major.minor.patch_ numbering.

It comes in handy, especially now that we have so many extensions, such as the new [WidgetKit](/2020/10/12/guide-to-widgetkit/) extension.

App Store requires the SAME version numbers for ALL of your targets. If you have been setting them manually, make use of Fastlane.

## 1. Set Marketing Version

Firstly, use `agvtool` to set the marketing version (`CFBundleShortVersionString`). This is required if you have never set it before.

For example, I set to "1.23" (using only major and minor).

    agvtool new-marketing-version 1.23

## 2. Add to Fastlane

Add the following to your Fastlane. This is a shortcut to generate 3 lanes.

```ruby
# Lanes: bump_major, bump_minor, bump_patch
%w{major minor patch}.each do |part|
  lane "bump_#{part}".to_sym do
    increment_version_number(bump_type: part)
  end
end
```

Now, to bump the minor, run `fastlane bump_minor`.

## 3. Also, bump build

For consistently, I also have a `fastlane bump` to bump the **build number**.

```ruby
lane :bump do
  increment_build_number
end
```
