---
layout: post
title: "Pitfall: Release Scheme optimizes code"
date: 2015-03-03 14:57:14 +0800
comments: true
categories: [iOS]
---

I was once writing a simple piece of code, something along the line of:

    NSString *name = [[NSBundle mainBundle] objectForInfoDictionaryKey:@"CFBundleDisplayName"];

It turns out strange that when I run in Release scheme, name is `nil` when I inspect the variable in debugger.

When I run in Debug scheme, it is fine. The code returns the app name as you expected.

<!-- more -->

## Pitfall

The pitfall is that when you run in Release mode, the [code is optimized](http://stackoverflow.com/a/13041747/242682).

The effect of the optimization is such that any information you see in debugger could not be the truth.

It might not be `nil`, though you see `nil` in debugger.

If you were to print out the variable at that breakpoint with lldb, it could say:

    Printing description of name:
    (NSString *) name = <no location, value may have been optimized out>

or 

    Printing description of name:
    (NSString *) name = <variable not available>

Both does not give you the true value of name. But at least they don't wrongly says the variable is `nil`..


## Solution

It you really want to debug in Release mode, you can change the optimzation level.

Go to **Project > App target > Build Settings > Optimization Level** and change to `None` for Release.

But remember to change it back for actual app release.

