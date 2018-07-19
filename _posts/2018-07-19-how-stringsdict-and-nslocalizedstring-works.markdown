---
layout: post
title: "How stringsdict and NSLocalizedString Works"
date: 2018-07-19T13:26:11+08:00
categories: []
---

When I wrote [everything about iOS localization](/2014/04/10/everything-about-ios-localization/), I covered on **plural support**, but was brief.

This post I will explain the use of the powerful `.stringsdict` with a simple use, explaining the basics, then an advanced use. Lastly I will try to explain some magic under the hood.

## Simple Case

It is easier to use an example and explain.

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
let numberOfFiles = 1
let format = NSLocalizedString("%d file(s) are selected", comment: "")
let localizedString = String(format: format, numberOfFiles)
```

There are 2 distinct calls here:

1. You need to get the "format" using the macro `NSLocalizedString`. In the code above, the variable format will be `%#@num_files_are@ selected`. Note: This is not yet localized, unlike the usual localization!
2. Init `String` with this format and the actual arguments, and you will get the localized string.

Basics of the XML:

- `%d file(s) are selected` is simply a key to refer to this 1 string to localize. But it is NOT necessary to have the `%d` in the key. You can rename the key as eg `files` and it will work. Having the `%d` is more of a good convention, telling us 1 number should be provided as an argument.
- `%#@num_files_are@ selected` is known as a **format**, which is made up of 1 **variable** `num_files_are` and the text " selected".
- `num_files_are` is a key (to the variable) with a dict to explain the **rules**:
  - `NSStringPluralRuleType` is plural rules (there could be [others](https://developer.apple.com/library/archive/documentation/MacOSX/Conceptual/BPInternational/StringsdictFileFormat/StringsdictFileFormat.html) in the future)
  - `NSStringFormatValueTypeKey` describe what type of argument works with this variable eg. `d` for `Int`
  - `zero` is a rule. When the argument is 0, it will use "No file is" to replace the variable `num_files_are`, therefore finally forming "No file is selected"
  - `one` is another rule
  - `other` is another rule. This time, it will use "%d files are", and the argument is used here, finally forming eg. "2 files are selected"

![stringsdict explained](/images/stringsdict-explained-basic.jpg)

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

It is an advanced use because the output depends on both the arguments.

The biggest difference is that this time, there are 2 variables, each with their set of rules, and one variable refer to another!

![Advanced stringsdict](/images/stringsdict-explained-advanced.jpg)

- The format is `%1$#@lu_completed_runs@`, which uses 1 variable `lu_completed_runs` (yes, just 1 variable in the format is okay), and that it is the first argument as specified by the `1$`
- In some of the rules for `lu_completed_runs`, it uses a **format** `%2$#@lu_total_runs@`, which uses another variable `lu_total_runs`. This variable is the 2nd argument as specified by the `2$`.
- The rules in `lu_total_runs` will produce part of the text for `lu_completed_runs`

There is some recursive lookup going on :)

## The Puzzling String

Let's look at the simple case again to explain something strange that goes on.

```swift
let format = NSLocalizedString("%d file(s) are selected", comment: "")
// format is "%#@num_files_are@ selected"
let str = String(format: format, 1)
// str is "1 file is selected"
```

If you have been using [`String(format:_:)`](https://developer.apple.com/documentation/swift/string/1417691-init), you know it can format the string and replace with the arguments.

But it is hard to digest for the format `%#@num_files_are@ selected`, a mere string.. How does a **mere string** have access to the rules in stringsdict?

The string `%#@num_files_are@ selected` is not a mere string. It knows the rules, for a particular locale.

Try this and it will NOT work:

```swift
// let format = NSLocalizedString("%d file(s) are selected", comment: "")
let format = "%#@num_files_are@ selected"
let str = String(format: format, 1)
```

This proves that the macro `NSLocalizedString`, which return a `String` via [`Bundle.localizedString`](https://github.com/apple/swift-corelibs-foundation/blob/master/Foundation/NSString.swift), is not a pure string.

I am puzzled and I hope someone can explain.

I can see  [`NSString`](https://github.com/apple/swift-corelibs-foundation/blob/master/Foundation/NSString.swift) use `CFStringCreateWithFormatAndArguments`. In [`CFString`](https://github.com/apple/swift-corelibs-foundation/blob/3a3da5261da739a20177d2438239143887889ac6/CoreFoundation/String.subproj/CFString.c), it mentions `stringsDictConfig`. It seems like the system refer to this dictionary to look up the rules and format the final string.
