---
layout: post
title: "Guide to UserNotifications Framework"
date: 2018-10-20T16:25:12+08:00
categories: [Notifications]
published: true
---

`UserNotifications` is a revamped framework for dealing with both local and remote notifications.

This post will cover the basic to creating local notifications.

## Authorization

The very first step is to ask for user's permission.

![](/images/usernotifications-alert.png)

```swift
let center = UNUserNotificationCenter.current() // I will be using center as a "shorthand" from now on
center.requestAuthorization(options: [.alert, .sound, .badge]) { granted, error in
    ...
}
```

New in iOS 12, you can [add a `provisional` option](/2018/10/08/new-in-unusernotificationcenter-for-ios-12/) to skip the system prompt, with some tradeoffs.

## Schedule a request

You create a [`UNNotificationRequest`](https://developer.apple.com/documentation/usernotifications/unnotificationrequest), which is made up of the content and trigger (see in next sections).

```swift
let request = UNNotificationRequest(identifier: UUID().uuidString, content: content, trigger: trigger)
center.add(request) { error in
    if let error = error { print(error) }
}
```

For the identifier, we give it a unique UUID, since we can't be bothered :D If you want to retrieve pending/delivered notifications later, you could use the same identifier.

### 1. Set the content

The [content](https://developer.apple.com/documentation/usernotifications/unnotificationcontent) is made up of a number of configurations:

```swift
let content = UNMutableNotificationContent()
content.title = "Reminder"
content.body = "Have you done that today?"
content.badge = NSNumber(value: "99")
content.sound = UNNotificationSound.default()
content.categoryIdentifier = "POOP-REMINDER"
```

It is much self explanatory, except that last line.

`categoryIdentifier` is an identifier string, which you have to registered to tell the system what action buttons is available along with the notification alert. Read the section on category later.

### 2. Set the trigger

The [trigger](https://developer.apple.com/documentation/usernotifications/unnotificationtrigger) is to configure the when/where to send the notifications.

`UNNotificationTrigger` is an abstract class, and you will need to use one of the 4 concrete trigger classes:

1. `UNTimeIntervalNotificationTrigger`
2. `UNCalendarNotificationTrigger`
3. `UNLocationNotificationTrigger`
4. `UNPushNotificationTrigger`

```swift
// Time interval
let trigger5minLater = UNTimeIntervalNotificationTrigger(timeInterval: 5*60, repeats: false)

// Calendar
var components = DateComponents()
components.hour = 8
let trigger8amEveryDay = UNCalendarNotificationTrigger(dateMatching: components, repeats: true)

// Location
let region = CLCircularRegion(center: center, radius: 2000.0, identifier: "YISHUN-WALL")
region.notifyOnEntry = true
region.notifyOnExit = false
let triggerByLocation = UNLocationNotificationTrigger(region: region, repeats: false)
```

## Category & Actions

A notification alert has associated action buttons, so that user can respond quickly.

The framework require the app to set up the "category", usually in your app delegate launch:

```swift
func setupNotificationCategory() {
    let deleteAction = UNNotificationAction(identifier: "DELETE", title: "Delete", options: .destructive)
    let addNewAction = UNNotificationAction(identifier: "NEW", title: "Add New"), options: .foreground)
    let reminderCategory = UNNotificationCategory(identifier: "REMINDER",
                                                  actions: [deleteAction, addNewAction],
                                                  intentIdentifiers: [])
    center.setNotificationCategories([reminderCategory])
}
```

Note that we register the **string identifier "REMINDER"**, which is the same string for `content.categoryIdentifier` in the earlier section.

You can create multiple categories with different action buttons, and of course with different identifier strings.

## Responding to user selected action

To handle, you have to implement `UNUserNotificationCenterDelegate`.

I usually do it in AppDelegate, so I will have `UNUserNotificationCenter.current().delegate = self` in app launch. It is up to you where to implement the delegate.

```swift
extension AppDelegate: UNUserNotificationCenterDelegate {
    func userNotificationCenter(_ center: UNUserNotificationCenter, didReceive response: UNNotificationResponse,
                                withCompletionHandler completionHandler: @escaping () -> Void) {
        switch response.actionIdentifier {
        case "NEW": ...
        case "DELETE": ...
        default: break
        }
        completionHandler()
    }
}
```

Oh, remember in `addNewAction` we use the option `foreground`? That is to tell the system to open the app and bring to foreground. So for the case "NEW", we could navigate the user some part of the app.

## Manage notifications

The framework provides API to manage all the the notifications. You can retrieve and delete via:

- `getDeliveredNotifications(completionHandler:)`
- `getPendingNotificationRequests(completionHandler:)`
- `removeAllDeliveredNotifications()`
- `removeAllPendingNotificationRequests()`
- `removeDeliveredNotificationRequests(withIdentifiers:)`
- `removePendingNotificationRequests(withIdentifiers:)`

## What's new in iOS 12

You can read in [another of my post](/2018/10/08/new-in-unusernotificationcenter-for-ios-12/) of the new stuff.
