---
layout: post
title: "Navigating Xcode 12 and Tabs"
date: 2020-08-21T22:47:10+08:00
categories: [Xcode]
---

Xcode 12 introduced a new tab system call **Document Tabs**. The old system is _Window Tabs_.

The new system screwed up many of the keyboard shortcuts I am used to.

One of the most troubling is that I can't create a tab with `⌘ T` anymore.

_NOTE: This is written as of Xcode 12 beta 5, so things might change. It also depends on Xcode Preferences > Navigation. I use the Open in Place and Uses Tab._

## Opening a file in document tabs

When you open a file, there is a new concept between 1) opening temporarily and 2) keeping in place.

`Click` a file to show in current editor. But this is only _temporarily_. If you click another file, it will replace the temporary file.

`Double click` or `⌘ ⌥ O` a file to keep in the editor.

Hence, my most common workflow now consists of 2 steps.

1. `⌘ ⇧ O` to quick search for a file
2. `⌘ ⌥ O` to keep it in place

Other usual shortcuts:

`⌘ ⇧ [` or `]` to navigate left and right of the tabs.

`⌘ W` to close a document tab.

## Window tabs

This seems like _deprecated_. Or we should try to minimize the use.

You cannot create a window tab using `⌘ T` anymore. If you want to, make sure `View > Show Window Tab Bar`, then click on the `+` on the right of the bar.

`⌘ ⇧ W` to close window tab.

`⌘ ⌥ ⇧ T` to rename a window tab.

## Make use of Editors

`⌥ Click` to open a file in the **split editor**.

`⌘ CTRL T` to open new editor.

`⌘ ⇧ CTRL W` to close split editor

`Drag` the tab to split editor, or to another window tab.

`⌥ Click` on a file to open in next editor.

`⇧ ⌥ Click` on a file, and move with mouse to an editor

## Other shortcuts

`⌘ ⇧ J` to locate the file in the project navigator

`CTRL 6` to dropdown the classes, methods and properties of a file. Following that, type to search further.

`⌘ ⇧ A` on a code under caret to show ⌥ions such as Callers, Rename, etc

Also [Xcode tips in 2012](/2012/09/26/xcode-4-dot-5-tips-and-tricks/).
