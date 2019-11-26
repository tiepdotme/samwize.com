---
layout: post
title: "Supporting Dark Mode in iOS 13"
date: 2019-11-26T12:00:42+08:00
categories: [iOS]
---

WWDC 2019 gave us **Dark Mode**. [NSHipster](https://nshipster.com/dark-mode/) has once again provided a complete guide, with a [helpful grouping](https://nshipster.com/dark-mode/#use-semantic-colors) of the new semantic colors.

While I am working on an app, I only have 2 questions.

## What about pre-iOS-13?

How will the new system color be interpreted on iOS 12, and earlier iOS?

If you select a system color in Xcode Interface Builder, then for pre-iOS-13, Xcode will use the default light mode color.

It is as if code with `#available` is written for you:

```swift
if #available(iOS 13.0, *) {
    theColor = .label
} else {
    theColor = .black
}
```

So in other words, it is **backward compatible**.

And so there's no reason not to provide dark mode, other than some tedious mapping of the semantic colors.

## About deprecated table group colors

If you use `groupTableViewBackground`, then you should replace with the new `systemGroupedBackgroundColor`.

The "group table" concept was introduced **since iOS 2**, but is now deprecated in iOS 13. The new concept dropped "group table", and simply refer to as "grouped background".

Table is dropped. That's the new new semantic.

It also makes perfect sense because often you will apply the color to non-table views.

Similarly, in the color selector, don't use the deprecated color.

![](/images/group-table-color-deprecated.jpg)

At the bottom of the screenshot, there is also a "Table Cell Grouped Background Color". That is replaced with "Secondary System Grouped Background Color".

###  Where are the grouped background being used?

1. "System Grouped Background" is the **table background**
2. "Secondary System grouped background" is the **table cell background**

I refer to the use in table, but it can be for collection view, or whatever kind of encapsulating groups.

There is also a "Tertiary" group, which would be for a group within the secondary group.

In light mode, (1) is light grey while (2) is white, which is the original group table.

In dark mode, (1) is black while (2) is dark grey.

Like this:

[![An Example of Baby Log app](/images/light-vs-dark-mode.jpg)](https://itunes.apple.com/app/id1065722243?at=11luru)
