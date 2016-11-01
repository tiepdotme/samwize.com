---
layout: post
title: "Guide to Installing KZPlayground/Code Injection for Xcode"
date: 2016-10-31T13:06:04+08:00
categories: [Xcode]
---

NOT WORKING (as of Nov 2016): [#171](https://github.com/johnno1962/injectionforxcode/issues/171)

Swift is terribly slow at compiling.

To save yourself the wait, there are great tools that toy around using a power technique: **code injection**

Prior to Xcode 8, things were easy with plugins.

But with Xcode 8, Apple now has officialy, yet limiting, way to develop for Xcode plugin, and code injection will not work unless Xcode is **unsigned**.


## Code Injection

[Code Injection](https://github.com/johnno1962/injectionforxcode) is a plugin to inject code while the app is running.

To use code injection, you have to unsign your Xcode.


## Unsign Xcode

[MakeXcodePluginsWork](https://github.com/nrbrook/MakeXcodePluginsWork), provides a way to unsign (and also to revert to signed) Xcode.

    git clone https://github.com/nrbrook/MakeXcodePluginsWork
    cd MakeXcodePluginsWork
    # Edit makeXcodePluginsWork if you Xcode path is different
    # Eg. My Xcode is /Applications/Xcode 8.app
    ./makeXcodePluginsWork

There [are](https://github.com/inket/update_xcode_plugins) [other](https://github.com/johntmcintosh/xcunsign) [tools](https://github.com/fpg1503/MakeXcodeGr8Again). All similarly uses steakknife's [unsign](https://github.com/steakknife/unsign).


## Build Code Injection Plugin

    git clone https://github.com/johnno1962/injectionforxcode
    open injectionforxcode/InjectionPluginLite/InjectionPlugin.xcodeproj

In Xcode, go to the target and make sure you select your signing team.

Pitfall: In Build Phases > Run Script, it will run codesign with "iPhone Developer". This might be ambiguous in some cases (at least for me with multiple account). If so, change the script to eg. "iPhone Developer: samwize".

Build the target.

Restart Xcode.

You should see "Unexpected Code Bundle ...". Load the Bundle.

In Xcode menu, you should see **Injection Plugin** and **Inject Source**.

Congrats! You are ready to inject code.


## Inject Your Code

Let's test by adding this special func to a view controller:

```swift
func injected() {
    print("I've been injected: \(self)")
}
```

This `injected` func is special as it is the entry point for any code injection. You can write your injected code within.

Run the target.

Then press `CTRL =` (or Product > Inject Source).

For more usage and limitations (especially for Swift), read the [project's README](https://github.com/johnno1962/injectionforxcode).


## KZPlayground

[KZPlayground](https://github.com/krzysztofzablocki/KZPlayground) makes use of code injection to create it's own Playground, for Swift and Objective-C, since it is just code injection.

It also provides control for tweaking stuff so you get a GUI like sliders.
