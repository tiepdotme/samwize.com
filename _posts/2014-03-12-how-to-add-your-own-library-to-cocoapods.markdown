---
layout: post
title: "How to add your own library to cocoapods"
date: 2014-03-12 20:51:31 +0800
comments: true
categories: [iOS]
---

[CocoaPods](http://cocoapods.org) is the de facto dependency manager for iOS/Mac development.

You can follow [this official guide](http://guides.cocoapods.org/making/making-a-cocoapod.html) to create your own pod so that other developers can use.

However, after following the official guide, I am rather lost.

<!-- more -->

I was stuck on multiple occassion, though still manage to resolve.

One stumbling block I had was their 1 liner instruction:

> Then submit a Pull Request to the Specs repo.

They could have elaborated to say "Fork Specs repo, add a directory with your podspec, then submit a pull request".

I was puzzled as I have a podspec from running `pod lib create`, and it don't make sense that I can submit a pull request between 2 different git projects.

Therefore, imo, this [comprehensive guide by Dave Meehan](http://davemeehan.com/technology/objective-c/cocoapods-creating-and-sharing-your-first-objective-c-open-source-project) is much better.

Anyway, here is my first cocoapod: [UIView+CustomFonts](https://github.com/samwize/UIView-CustomFonts)

My brief steps:

- Run `pod lib create UIView+CustomFonts`
- Create an Xcode project, and then move it to the same directory "UIView+CustomFonts"
- The main files for the libraries are in "Classes"
- Edit podspec
- Run `pod spec lint` to check the syntax
- Create an empty repos in github, add the remote, then push
- Fork [Specs repo](https://github.com/CocoaPods/Specs)
- Create directory "/UIView+CustomFonts/1.0.0/"
- Copy (yes, a copy) the current podspec into the created directory
- Submit pull request to [Specs repo](https://github.com/CocoaPods/Specs)

In the end, I realize I changed so much from the initial `pod lib create`.. If I will to start a new library, I will probably download from [UIView+CustomFonts](https://github.com/samwize/UIView-CustomFonts) and copy the structure.
