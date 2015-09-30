---
layout: post
title: "New CocoaPods hang when pod install/update"
date: 2014-10-14 18:43:25 +0800
comments: true
categories: 
---

CocoaPods kept having breaking changes.

This time, I updated to 0.34.2, and something broke, again.

<!-- more -->

I was using a private repository to a git URL directly (without a private spec repo).

Hence, this time I [created my private spec repo](http://guides.cocoapods.org/making/private-cocoapods.html), and then [push my library podspec](http://guides.cocoapods.org/making/making-a-cocoapod.html) to the spec repo.

Along the way, there are hiccups.

Here are some randoms tricks that solve some weird issues.


## Use Git SSH

When I use git https, it hangs on cloning the repos. I turned on verbose mode to find out it hangs on `git clone ... --single-branch --depth 1`

I have to switch from using 'https' to 'ssh'.

And it no longer hangs.



## lint spec warnings

[Strangely](https://github.com/CocoaPods/CocoaPods/issues/460), `pod repo push` will have validation errors even if it has warnings!

The solution is to have `--allow-warnings` flag.

With that, you can push your podspec with:

    pod repo push SPEC_NAME POD_NAME.podspec --verbose --allow-warnings



## Missing lpod

For error "library not found for lpods-xxx", you might be able to [solve](http://samwize.com/2014/05/15/resolving-cocoapods-build-error-due-to-targets-building-for-only-active-architecture/) with in podfile:

    # Most lib require to build for all arch
    post_install do |installer_representation|
        installer_representation.pods_project.targets.each do |target|
            target.build_configurations.each do |config|
                config.build_settings['ONLY_ACTIVE_ARCH'] = 'NO'
            end
        end
    end


## JSON or Ruby podspec

It used to be all podspec are in ruby format, until they converted the master trunk to json format (automatically).

You should still be writing in ruby format.

But if you have have a json format in your private spec repo, [that is fine](https://github.com/CocoaPods/CocoaPods/issues/2320) too.


## Issue with linking to pod framework

I was using MillennialMedia pod, and their library includes a framework.

Unfortunately, somehow, my project (or MoPub Pod) could not link their framework correctly, even though the framework is indeed build. 

What I did is mannually add the framework to Pods project, and also make sure MoPub target membership includes MillennialMedia pod.


