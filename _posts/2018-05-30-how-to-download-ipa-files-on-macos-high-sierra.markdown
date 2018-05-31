---
layout: post
title: "How to Download IPA Files on macOS High Sierra"
date: 2018-05-30T22:16:00+08:00
categories: [Tips]
---

macOS, or more specifially since iTunes v12.7, no longer stores the IPA files.

There are a number of ways to find an app's IPA, such as the many untrusted, dangerous sites with _their signed IPA_.

Be safe, read on.

## 1. Download Apple Configurator 2

This is Apple's app for configuring multiple devices for schools and businesses.

Download it from Mac Store [here](https://itunes.apple.com/app/id1037126344?at=11luru).

## 2. Sign In

Go to the app > Account > Sign In using your personal iTunes account.

## 3. Update The Apps

Go to Action > Update, and select the apps to update (or all).

## 4. Find in folder

The IPAs will be here:

`~/Library/Group Containers/K36BKF7T3D.group.com.apple.configurator/Library/Caches/Assets/TemporaryItems/MobileApps`

Note that they are in a **cache**, which means it will be deleted anytime, so copy it to somewhere while it is there.
