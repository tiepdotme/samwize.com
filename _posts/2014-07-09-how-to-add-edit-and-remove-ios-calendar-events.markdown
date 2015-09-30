---
layout: post
title: "How to add, edit and remove iOS calendar events"
date: 2014-07-09 07:03:16 +0800
comments: true
categories: iOS
---

You can access a user calendar to insert new event, or to edit or delete.

Some code snippets:

<!-- more -->

## Import EventKit

```objc
#import <EventKit/EventKit.h>
```

## Add Event

```objc 
EKEventStore *store = [[EKEventStore alloc] init];
[store requestAccessToEntityType:EKEntityTypeEvent completion:^(BOOL granted, NSError *error) {
  if (!granted) return;
  EKEvent *event = [EKEvent eventWithEventStore:store];
  event.title = @"Event Title";
  event.startDate = [NSDate date]; // today
  event.endDate = [event.startDate dateByAddingTimeInterval:60*60];  // Duration 1 hr
  [event setCalendar:[store defaultCalendarForNewEvents]];
  NSError *err = nil;
  [store saveEvent:event span:EKSpanThisEvent commit:YES error:&err];
  NSString *savedEventId = event.eventIdentifier;  // Store this so you can access this event later
}];

```

## Edit Event

```objc 
EKEventStore *store = [[EKEventStore alloc] init];
[store requestAccessToEntityType:EKEntityTypeEvent completion:^(BOOL granted, NSError *error) {
  if (!granted) return;
  EKEvent *event = [store eventWithIdentifier:savedEventId];
  // Uncomment below if you want to create a new event if savedEventId no longer exists
  // if (event == nil)
  //   event = [EKEvent eventWithEventStore:store];
  if (event) {
    NSError *err = nil;
    event.title = @"New event title";
    [store saveEvent:event span:EKSpanThisEvent commit:YES error:&err];
  }
}];
```


## Delete Event

```objc
EKEventStore *store = [[EKEventStore alloc] init];
[store requestAccessToEntityType:EKEntityTypeEvent completion:^(BOOL granted, NSError *error) {
  if (!granted) return;
  EKEvent* eventToRemove = [store eventWithIdentifier:savedEventId];
  if (eventToRemove) {
    NSError* err = nil;
    [store removeEvent:eventToRemove span:EKSpanThisEvent commit:YES error:&err];
  }
}];
```
