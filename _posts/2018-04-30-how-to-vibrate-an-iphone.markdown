---
layout: post
title: "How to Vibrate an iPhone"
date: 2018-04-30T09:49:03+08:00
categories: []
---

## Steps

1. Target > Linked Frameworks and Libraries > Add **AudioToolbox**

2. `import AudioToolbox`

3. `AudioServicesPlaySystemSoundWithCompletion(kSystemSoundID_Vibrate, nil)`

It is that simple using [AudioServicesPlaySystemSound API](https://developer.apple.com/documentation/audiotoolbox/1405248-audioservicesplaysystemsound).


## Pitfall: Does not work along with other audio session

There is a [note](https://developer.apple.com/documentation/audiotoolbox/1405202-audioservicesplayalertsound):

>  However, the device does not vibrate if your app’s audio session is configured with the AVAudioSessionCategoryPlayAndRecord or AVAudioSessionCategoryRecord audio session category.

While the note is for `AudioServicesPlayAlertSound`, it is applicable to `AudioServicesPlaySystemSound` too.

As long as you have a audio session (via AVAudioPlayer, AVCaptureMovieFileOutput, etc), then the phone will NOT vibrate.

To make it work, use the new Haptic API.

## Haptic API

In iOS 10, there is a new API, making use of the new haptic engine in iPhone.

The API is very simple, with 3 concrete classes to [UIFeedbackGenerator](https://developer.apple.com/documentation/uikit/uifeedbackgenerator). Use accordingly to your scenario.

```swift
    // UI "impact"
    UIImpactFeedbackGenerator(style: .light).impactOccurred()
    UIImpactFeedbackGenerator(style: .medium).impactOccurred()
    UIImpactFeedbackGenerator(style: .heavy).impactOccurred()

    // Selection changed
    UISelectionFeedbackGenerator().selectionChanged()

    // Notifications
    UINotificationFeedbackGenerator().notificationOccurred(.success)
    UINotificationFeedbackGenerator().notificationOccurred(.error)
    UINotificationFeedbackGenerator().notificationOccurred(.warning)

    // To be complete, this is the vibration using AudioToolbox
    AudioServicesPlaySystemSoundWithCompletion(kSystemSoundID_Vibrate, nil)
```
