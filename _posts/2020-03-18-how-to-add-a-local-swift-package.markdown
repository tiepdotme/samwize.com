---
layout: post
title: "How to Add a Local Swift Package"
date: 2020-03-18T10:48:11+08:00
categories: [Swift]
---

[WWDC 2019 Session 410](https://developer.apple.com/videos/play/wwdc2019/410/) covered how you can create your own library. In the video you'll see how the package is a folder that can be dragged around, and that once it is in a git repo, you can add it to other projects.

But what if you want to link to a local library?

The steps are seemingly easy:

1. Drag the package **folder** into your project
2. In project target > add framework > select the local package

## PITFALL: You can only open 1 project

The pitfall is that you cannot have multiple projects (that include the same local package) opened at the same time.

If you open multiple projects, the local package will NOT be expandable in the project navigator, nor can it be added as a framework.

Hopefully Xcode can fix that.
