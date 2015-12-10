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
