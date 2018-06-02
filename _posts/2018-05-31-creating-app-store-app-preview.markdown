---
layout: post
title: "Creating App Store App Preview"
date: 2018-05-31T17:03:44+08:00
categories: []
published: false
---


https://developer.apple.com/app-store/app-previews/

- [Video specs](https://help.apple.com/itunes-connect/developer/#/dev4e413fcb8)
- 2 device form: iPhone & iPad
  - Use any 1 iPhone, any 1 iPad
- Either portrait or landscape
- Must be between 15-30 sec
- Up to 3 app previews
- Support localization

## Tips

- Start with iPhone 8 Plus 5.5", that is resolution 1080x1920
- Most phones are muted, so video should be presentable even without sound. Add text in between transition of screens.
- By right (in ethical sense), you are supposed to display a text disclaimer when showing features only available via in-app purchases.

Some [examples](https://medium.com/@incipiagabe/10-examples-of-app-store-preview-videos-f8d669e586c8)

## Have a Plan

- Define the goal eg. user will be impressed by xxx
- Write an outline or a storyboard, with the timings
- Your screenshots can be a good starting point

## Record on simulator



    xcrun simctl io booted recordVideo example.mp4


    # If somehow not 30fps
    ffmpeg -i "original.mov" -r 30 "converted_30fps_video.mov"
