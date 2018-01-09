---
layout: post
title: "What I Know About Provisioning Profile"
date: 2015-12-10T12:04:26+08:00
categories: [iOS]
---

All Apple developers have to face the [mystical provisioning profile](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppStoreDistributionTutorial/CreatingYourTeamProvisioningProfile/CreatingYourTeamProvisioningProfile.html).

It is a complex creature that I dealed with frequently, ever since my first encounter with iOS development.

I have never fully understood why/how a provisioning profile is needed when submiting an app. I am not an expert, but here I will provide a few things I know.

## What is Provisioning Profile

[bigzaphod](http://bigzaphod.tumblr.com/post/78574849549/provisioning) wrote a good post. 

In short:

> Provisioning profiles are where the vasty majority of the confusion is. They represent a union of almost everything mentioned so far and act as a single solution that addresses several issues. The important thing to remember is that they exist to grant an iOS device permission to grant your app specific permissions.

A provisioning profile includes _almost everything_ -- App ID, Team ID, developer cert, entitlements, devices (for Adhoc).

It is needed to securely sign your app. It works because Apple generates the provisioning profiles (signed by Apple's own private key), and you in turn sign your app with the provisioning profile.

## Quick Look

A good tool is [Provisioning](https://github.com/chockenberry/Provisioning), which let you quick look the details of a `.mobileprovision` file.

You get a better idea of what a profile contains.

## Fastlane sigh

Working with provisioning profile is tedious because you have to manage a lot of profiles. 

Each app could require up to 3 types of profile -- Development, Adhoc and App Store Distribution.

You need to download from Apple Developer Center, then install them.

And they expire in one year, so you have to regenerate rather frequently.

[`sigh`](https://github.com/fastlane/sigh) takes care of that.

    # Create an App Store Distribution profile, download and install
    sigh

    # For Adhoc
    sigh --adhoc

    # For Development
    sigh --development

You can also manage all profiles that were installed locally via `sigh manage`, and delete the expired one with `sigh manage -e`. It's handly when you have hundreds of profiles.

> Because you would rather spend your time building stuff than fighting provisioning

## Project Schemes & Configurations

Your project should have 2 configurations (by default). Go to Project > Info > Configurations:

1. Debug
2. Release

Configurations are basically for different build settings, etc.

By default, Xcode will enable "Automatically manage signing". My preference is to use Fastlane match, disable this setting, and manually select the match's provisioning profiles: 

1. Signing (Debug) -- "match Development com.just2us.myproject"
2. Signing (Release) -- "match AppStore com.just2us.myproject"

By default, your project has 1 scheme. Edit Scheme, and you should see:

- Under Run, Build Configuration is "Debug", therefore signs with development provisioning profile.
- Under Archive, Build Configuration is "Release", therefore signs with distribution provisioning profile.

But after you archive, you may still re-sign with another provisioning profile eg. Ad Hoc!

![Exporting an archive](https://developer.apple.com/library/content/documentation/IDEs/Conceptual/AppDistributionGuide/Art/6_ios_createappstorepackage_1_2x.png)

## Fastlane gym

Fastlane gym makes building a Xcode project easy. You could provide options to manually select the provisioning profiles and the export method.

```ruby
gym(
  workspace: "MyProject.xcworkspace",
  scheme: "MyProject",
  export_options: {
    provisioningProfiles: { 
      "com.just2us.myproject"" => "match AdHoc com.just2us.myproject""
    }
  },
  skip_profile_detection: true,
  export_method: 'ad-hoc'
)
```

Though using Fastlane match, it should be able to auto detect, but you need to add a **new** project configuration -- I call mine "Staging" -- and select the provisioning profile for **Signing (Staging)** respectively, then gym with the new configuration like this:

```ruby
gym(
  workspace: "MyProject.xcworkspace",
  scheme: "MyProject",
  configuration: "Staging",
  export_method: 'ad-hoc'
)
```
