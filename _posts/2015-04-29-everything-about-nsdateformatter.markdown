---
layout: post
title: "Everything About NSDateFormatter"
date: 2015-04-29 16:27:45 +0800
comments: true
categories: [iOS]
published: false
---

When it comes to dealing with date, it is never simple.

The [Apple programming guide](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/DataFormatting/Articles/dfDateFormatting10_4.html) has a chapter on NSDateFormatter, but it is not clear enough.


<!-- more -->

## The Format

Firstly, refer to [unicode.org for the date format pattern](http://unicode.org/reports/tr35/tr35-6.html#Date_Format_Patterns).  It is so important a document that you should bookmark it.

Some common patterns:



## Escaping

To escape in the pattern string, you can enclose with single quote.

For example, to escape the character "T", you use `'T'HH:mm:ss`. The pattern produces eg. **T09:30:00**



## 'Z' is for Zulu Time

Another common format is `yyyy-MM-dd'T'HH:mm:ss'Z'`

Eg. **2015-04-18T01:32:40.123Z**

The `Z` at the end [refers](http://stackoverflow.com/questions/9706688/what-does-the-z-mean-in-unix-timestamp-120314170138z) to Zulu time, which is also GMT and UTC. It is the RFC 3339 format.

Zulu sounds like zero - the zero hours.


## NSDate

Important: `NSDate` don't have a time zone.



## NSCalendar


## NSTimeZone

Print out all the supported time zone with:

    NSLog(@"%@", [NSTimeZone knownTimeZoneNames]);

If you know a name, you can init a timezone easily:

    [NSTimeZone timeZoneWithName:@"Asia/Singapore"];

You can also refer to wiki for [a list of the timezone names](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones).


## YLMoment

https://github.com/yannickl/YLMoment
