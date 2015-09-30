---
layout: post
title: "My Custom Localization and genstrings"
date: 2012-11-06 21:49
comments: true
categories: iOS
---

This post is about some custom stuff I did for [localization](https://developer.apple.com/library/ios/#documentation/MacOSX/Conceptual/BPInternational/Articles/ChoosingLocalizations.html). 

It does not cover the [basic](http://www.raywenderlich.com/2876/how-to-localize-an-iphone-app-tutorial) of how you localized an app, nor how to use the Apple provided [genstrings](http://spritebandits.wordpress.com/2012/01/25/ios-iphone-app-localization-genstrings-tips/) tool.

<!-- more -->

I have some particular bad vibes with using `NSLocalizedString`. I hate it being 17 characters long, and the REQUIRED comment parameter that you MUST pass in, which usually is just `nil` for me.

And so I have my own Macro called `Localized`.

Which I improved to read from other table strings, and has an awesome recursive replacement.

By doing that, I lose the use of `genstrings`, as it no longer uses `NSLocalizedString`. Unfortunately `genstrings` is closed source. But fortunately, some [python helper](https://github.com/dunkelstern/Cocoa-Localisation-Helper) is around.

And the result is at [https://github.com/samwize/localized](https://github.com/samwize/localized).