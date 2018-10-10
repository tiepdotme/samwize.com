---
layout: post
title: "New in UNUserNotificationCenter for iOS 12"
date: 2018-10-08T15:25:57+08:00
categories: [Notifications]
---

`UNUserNotificationCenter` is the revamped notification framework, deprecating `UILocalNotification` since iOS 10.

In WWDC 2018, there are some nice features in the framework, and here I will only be covering what's new.

## Provisional

Prior to iOS 12, an app has to [explicitly ask user for permission](https://developer.apple.com/documentation/usernotifications/asking_permission_to_use_notifications) before sending any notifications. Users will have to allow via a system alert like this:

![](/images/usernotifications-alert.png)

With the `provisional` option, the alert will NOT be shown!

```swift
UNUserNotificationCenter.current().requestAuthorization(options: [.alert, .sound, .badge, .provisional]) { ... }
```

This is great as users will automatically receive the notification, with the following tradeoff:

The **notification will be delivered quietly**, which means they will only appear in notification center, but NOT in the lock screen, present banners, update badge, or play sound.

![](/images/usernotifications-quiet.png)

This is good enough for many apps.

Subsequently, users can opt in to receive prominent notifications by going to iOS Settings and enable for Lock Screen and Banners, or when user choose **Keep**, they will be able to choose between quiet or prominent mode.

![](/images/usernotifications-keep-or-not.png)

Or you can request authorization again, this time without `provisional`.

```swift
UNUserNotificationCenter.current().requestAuthorization(options: [.alert, .sound, .badge]) { ... }
```

## Critical Alert

All notifications are delivered silently if user enable **Do Not Disturb** mode.

All notifications are also delivered without sound if mute switch is on.

To override those, you can use the `criticalAlert` option. But this require special entitlement, and you need to fill [this form](https://developer.apple.com/contact/request/notifications-critical-alerts-entitlement/).

The kind of app Apple enable for are health and home security apps.

## Custom App Settings Link

If your app provide your own UI settings for configuring notifications, you can provide the option `providesAppNotificationSettings` so that iOS will link to your app UI in the following 2 places:

1. In the "Manage" option when user receive a notification
2. In iOS Settings > your app > Notification

[MacKuba](https://mackuba.eu/2018/06/11/notifications-in-ios-12/) has the screenshots if you are unsure.

To provide your custom UI, implement in your app delegate:

```swift
func userNotificationCenter(_ center: UNUserNotificationCenter, openSettingsFor notification: UNNotification?) {
    // Display your custom UI
}
```

## Other Basics

~~I have never cover the framework, and won't be for now :)~~ I have a [guide on the basics to local notifications](/2018/10/20/guide-to-usernotifications-framework/).

The Apple's documentation is also pretty good, with the following topics:

1. [Asking permission](https://developer.apple.com/documentation/usernotifications/asking_permission_to_use_notifications)
2. [Custom actions](https://developer.apple.com/documentation/usernotifications/declaring_your_actionable_notification_types) with buttons
3. [Handling received notifications](https://developer.apple.com/documentation/usernotifications/handling_notifications_and_notification-related_actions)
4. [Schedule local notifications](https://developer.apple.com/documentation/usernotifications/scheduling_a_notification_locally_from_your_app)
