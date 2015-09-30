---
layout: post
title: "Xcode 4.5 Tips &amp; Tricks"
date: 2012-09-26 01:16
comments: true
categories: [iOS, Mac]
published: true
---

Some of my favorite Xcode tips & tricks (tested on Xcode 4.5).

## Shortcut ##

{% img center /images/xcode-shortcuts.png xcode shortcuts %}

<!-- more -->

## Catch All Exceptions ##

1. Bring up breakpoint navigator (⌘6)
2. Click + on the bottom left
3. Add Exception Breakpoint

{% img center /images/xcode-catch-all-exceptions.png xcode catch exceptions%}

_Anyone knows the difference with [symbolic breakpoint](http://blog.just2us.com/2012/02/find-the-real-exception-in-xcode-debugger/)?_


## NSLog, Auto Continue on Breakpoints ##

Breakpoints are so much powerful. Don't limit to just pause on every breakpoint.

You could set actions such as log a message under certain condition, or automatically continue. Or if you are not interested in the first 10 times of a while loop, you can ignore x times before pause. Or even have your Mac speaks out instead of traditional text logging.

Edit a breakpoint for more options:

{% img center /images/xcode-nslog-on-breakpoints.png breakpoints actions %}


## Use of Tabs ##

1. Create new tabs with ⌘T
2. Double click to edit tab name
3. My workflow uses these tabs: Project Resources, Design, Coding 1, Coding 2, Build

{% img center /images/xcode-tabs-workflow.png tabs for my workflow %}

It's totally up to you to organize your tabs, just like web browsing.

## Use of Behaviours ##

Behaviours are hooks to Xcode for events like build, test, run, and search. You can find them in **Preferences** > **Behaviours**.

These are my additional behaviours.

Show Build tab when there is new issue:

{% img center /images/xcode-behaviour-build-new-issues.png 'Show Build tab when there is new issue' %}

Show console when start running:

{% img center /images/xcode-behaviour-console.png 'Show console when start running' %}

Show Debug tab when paused:

{% img center /images/xcode-behaviour-debug-tab.png 'Show Debug tab when paused' %}


## More Reference ##

1. [WWDC 2012](http://developer.apple.com/videos/wwdc/2012/) - "Working Efficiently with Xcode" is an excellent introductory
2. [Xcode4 User Guide](http://developer.apple.com/library/ios/#documentation/ToolsLanguages/Conceptual/Xcode4UserGuide/000-About_Xcode/about.html#//apple_ref/doc/uid/TP40010215-CH1-SW1) - Complete, but way too much text
3. [miso blog](http://blog.gomiso.com/2012/02/07/work-efficiently-with-xcode/) - from a developer