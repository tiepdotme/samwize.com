---
layout: post
title: "How stringsdict and NSLocalizedString Works"
date: 2018-07-19T13:26:11+08:00
categories: []
published: false
---

When I wrote [Everything About iOS Localization](/2014/04/10/everything-about-ios-localization/), I covered briefly on **plural support** using a `.stringsdict` file (instead of regular `.strings`).

This stringsdict turned out to be very powerful, and quite many under-the-hood stuff.

## Simple Case

It is easier to use an example and explain they are doing.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>%d file(s) are selected</key>
    <dict>
        <key>NSStringLocalizedFormatKey</key>
        <string>%#@num_files_are@ selected</string>
        <key>num_files_are</key>
        <dict>
            <key>NSStringFormatSpecTypeKey</key>
            <string>NSStringPluralRuleType</string>
            <key>NSStringFormatValueTypeKey</key>
            <string>d</string>
            <key>zero</key>
            <string>No file is</string>
            <key>one</key>
            <string>A file is</string>
            <key>other</key>
            <string>%d files are</string>
        </dict>
    </dict>
</dict>
</plist>
```

The code to use:

```swift
let format = NSLocalizedString("%d file(s) are selected", comment: "")
let str = String(format: format, 1)
```

There are 2 distinct calls here:

1. You need to get the "format" using the macro `NSLocalizedString`. In this case, the format will be `%#@num_files_are@ selected`. Note: This is not yet localized!
2. Init the `String` with this format and the actual argument (an Int for number of files). This initialization will keep start the localization.

For the curious people: If you are scratching your head on (2), thinking how on earth a string initialization will somehow consume the stringsdict and localize the text, you can read the last section.

Let's go back to explain the stringsdict:

- The key `%d file(s) are selected` is simply a key to refer to this string to localize. While we did use 1 argument, it is NOT necessary to have the `%d` in the key, but we have it (a convention to speak) because it explain that 1 Int is needed for the argument.
- The format `%#@num_files_are@ selected` is made up of 1 variable `num_files_are` and the text " selected".
- The key `num_files_are` explains how the variable in the format (read above point) works, with a dict:
  - Plural rules use `NSStringPluralRuleType`, but there are others
  - Along with `NSStringFormatValueTypeKey` with `d` for `Int`
  - `zero` is when the argument is 0, and it will use "No file is" to replace the variable `num_files_are`, therefore finally forming "No file is selected"
  - Similarly for `one`
  - For `other`, it will use "%d files are", and the first argument is used here, finally forming eg. "2 files are selected"

## Advanced Case

I like the example used in [objc.io](https://www.objc.io/issues/9-strings/string-localization/#localized-format-strings):

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>scope.%lu out of %lu runs</key>
    <dict>
        <key>NSStringLocalizedFormatKey</key>
        <string>%1$#@lu_completed_runs@</string>
        <key>lu_completed_runs</key>
        <dict>
            <key>NSStringFormatSpecTypeKey</key>
            <string>NSStringPluralRuleType</string>
            <key>NSStringFormatValueTypeKey</key>
            <string>lu</string>
            <key>zero</key>
            <string>No runs completed yet</string>
            <key>one</key>
            <string>One %2$#@lu_total_runs@</string>
            <key>other</key>
            <string>%lu %2$#@lu_total_runs@</string>
        </dict>
        <key>lu_total_runs</key>
        <dict>
            <key>NSStringFormatSpecTypeKey</key>
            <string>NSStringPluralRuleType</string>
            <key>NSStringFormatValueTypeKey</key>
            <string>lu</string>
            <key>one</key>
            <string>run completed</string>
            <key>other</key>
            <string>of %lu runs completed</string>
        </dict>
    </dict>
</dict>
</plist>
```

The localized string requires 2 arguments:

```swift
let completedRuns = 1
let totalRuns = 2
let format = NSLocalizedString("scope.%lu out of %lu runs", comment: "")
let str = String(format: format, completedRuns, totalRuns)
```

Let's look at the output first.

```
Completed runs    Total Runs    Output
------------------------------------------------------------------
0                 0+            No runs completed yet
1                 1             One run completed
1                 2+            One of x runs completed
2+                2+            x of y runs completed
```

- The format is `%1$#@lu_completed_runs@`, which uses 1 variable `lu_completed_runs`, and that is the first in the argument as specified by the `1$`
- The dict in `lu_completed_runs` gives the rule, and it does so using another format `%2$#@lu_total_runs@` (in only the rules "one" and "other"). The rules on the use of the variable `lu_total_runs` is in another dict. And this variable is the 2nd argument.
- The dict in `lu_total_runs` explains the rules for the second arguments

There is some recursive lookup going on :)

## Puzzling String

Let's look at the simple case again to explain something strange going on.

```swift
let format = NSLocalizedString("%d file(s) are selected", comment: "")
// format is "%#@num_files_are@ selected"
let str = String(format: format, 1)
// str is "1 file is selected"
```

If you have been using [`String(format:_:)`](https://developer.apple.com/documentation/swift/string/1417691-init), you know it can format the string and replace with the arguments.

But the format %#@num_files_are@ selected" is hard to digest on how it repalce with the arguments.

[`String`](https://github.com/apple/swift-corelibs-foundation/blob/master/Foundation/String.swift) is bridged with [`NSString`](https://github.com/apple/swift-corelibs-foundation/blob/master/Foundation/NSString.swift), which uses `CFStringCreateWithFormatAndArguments`.

In the initalization of `NSString`, you can provide a locale object (default to `Locale.current`).

```swift
public convenience init(format: String, locale: AnyObject?, arguments argList: CVaListPointer)
```

In [`CFString`](https://github.com/apple/swift-corelibs-foundation/blob/3a3da5261da739a20177d2438239143887889ac6/CoreFoundation/String.subproj/CFString.c), you will find a mention of `stringsDictConfig`.

It seems like the system refer to this dictionary to look up the rules and format the final string.
