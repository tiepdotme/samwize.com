---
layout: post
title: "React Native for iOS Swift Developer"
date: 2017-01-09T10:20:38+08:00
categories: [react]
---

React Native is the biggest technology available for iOS developer, since 2015.

It provides a new platform for iOS development, in the very popular (and familar language for web developers) - **JavaScript**.

This post is about learning/considering React Native for an iOS/Swift developer.

This post is not a development guide, because the official [Getting started](https://facebook.github.io/react-native/releases/next/docs/getting-started.html) is good, and [Ray Wenderlich](https://www.raywenderlich.com/136047/react-native-existing-app) provided a great guide for Swift iOS developers.

Instead, I will highlight important considerations for Swift iOS developers.


## Why use React Native?

Instead of pure iOS/Android native development, React Native has the following benefits:

1. Render native UI (NOT web view), thus good user experience
2. Learn once, write anywhere (on both iOS & Android)
3. Similar set of technology stack (JavaScript + React + millions of JS libs) with web developers 
4. JavaScript developers readily available, as compared to Swift/Android/Java

I usually do not advocate non-native technology, because there are limitations and extra bugs because of the intermediary platform.

But React Native is created by Facebook, and they use it in Facebook app too, so there is lesser risk. 

_Not zero risk, because you should know Facebook [kills Parse](http://blog.parse.com/announcements/moving-on/) etc too.._


## Warning: JavaScript World

Before we dived into React Native, let me warn you if you are a JavaScript newbie like me.

**JavaScript world is very messy.**

How messy? See this [conversation](https://hackernoon.com/how-it-feels-to-learn-javascript-in-2016-d3a717dd577f).

The ecosystem is _too healthy_, with lots of JS frameworks popping up and taking over the world, quickly. Check out the [state of JS in 2016](http://stateofjs.com/). There will be many frameworks that you need to learn, from everyone.


## How React Native Works?

In Facebook's [March 2015](https://code.facebook.com/posts/1014532261909640/react-native-bringing-modern-web-techniques-to-mobile/) release post:

> The only difference in the mobile environment is that instead of running React in the browser and rendering to divs and spans, we run it in an embedded instance of JavaScriptCore inside our apps and render to higher-level platform-specific components.

[JavaScriptCore](http://nshipster.com/javascriptcore/) is a framework provided on iOS/Android to run JavaScript.

React Native runs the JavaScript in JavaScriptCore, and then render the native UI views on the platform.


## The Trigger Point

In AppDelegate, there is boilerplate code that specify where to load the JavaScripts.

The `NSURL` either points to:

1. http://localhost:8081 or
2. NSBundle

Loading JavaScript remotely from a URL is interesting, because then, it is possible to [code push](https://microsoft.github.io/code-push/) to live apps!


## Flexbox for UI layout

No more messing around with autolayout.

[Flexbox](https://facebook.github.io/react-native/docs/flexbox.html) is a simple way, specifying a weight value for the components, the flow direction, and the alignment.

You will be glad that Boostrap 4 will use flexbox way, instead of the regular grid system they started with.


## Persistant Storage

You can use [AsyncStorage](https://facebook.github.io/react-native/docs/asyncstorage.html) for simple key-value data, just like `NSUserDefault`.

For database, you could use [Realm](https://realm.io/news/introducing-realm-react-native/)! No more Core Data!


## Migrating an existing Swfit project

You can have a hybrid of native and react native.

To integrate react native into an existing project, follow their [guide](https://facebook.github.io/react-native/docs/integration-with-existing-apps.html).

However, note their guide is outdated for the dependencies version. As of Dec 2016, I updated the version to:

```
"dependencies": {
  "react": "15.3.1",
  "react-native": "0.34.0"
}
```

You can update accordingly, but things move fast so if your project doesn't compile, then it could be the versions (and Xcode/iOS) incompatibility.


## Navigation

Let's take a look on a very basic UI subject. In iOS, we have `UINavigationController`.

In React Native, it gets complicated and there are more choices:

- Facebook's out of the box [Navigator](https://facebook.github.io/react-native/docs/navigator.html)
- Facebook's abandoned [NavigatorIOS](https://facebook.github.io/react-native/docs/navigatorios.html)
- WIX's [react-native-navigation](https://github.com/wix/react-native-navigation)

There are lots of community contributions, and once you found the library that is suitable for your requirement, you could use it easily.


## macOS too

Not only for mobile, but you could learn once, and write for [desktop app](https://github.com/ptmt/react-native-macos) too.


## IDE

Facebook has their own "IDE" - [nuclide](https://nuclide.io) - which is [plugin for Atom](https://atom.io/packages/nuclide).


## What about type?

One of the cons of JavaScript (compared to Swift) is that there is no type safety built into the language.

If you prefer to deal with types (which you should, trust me), then you should use [Flow](https://flowtype.org).

    # Install flow globally so that it is added to PATH
    npm install -g flow-bin

I would recommend to setup Flow when you begin a project, because it gets harder to migrate _non-flow app_ later on.


## What about TypeScript?

TypeScript provides typing and optionals, in a new language of it's own.

I would choose to use TypeScript, if I had more experience with JavaScript (especially with the new ES2015). But being a newbie to JavaScript, it is better to learn the intricates of the language.

What's more, most examples and libraries will be in pure JavaScript.

And compared to Flow, Facebook has [preference](https://github.com/facebook/react-native/issues/2502) for Flow. The react library has Flow annotations.

So my advise is to avoid TypeScript. But it is [possible](https://raygun.com/blog/2016/07/react-native-typescript/) [to use](https://medium.com/react-weekly/react-native-and-typescript-ad57b7413ead), just saying.


## Project Generator

[ignite](https://github.com/infinitered/ignite) is extension of [yeoman](http://yeoman.io). 

Generate a best-practised project structure with state of the art setup.


## What is not so great?

- Dependency on another technology stack
- You will not be able to use the latest iOS technology
- Swift is nicer than JavaScript


## More Resources

[awesome-react-native](https://github.com/jondot/awesome-react-native) - List of great libraries

[F8 - Facebook conference app](https://github.com/fbsamples/f8app) - written in react native, fully open sourced

[js.coach](https://js.coach/react-native) - Discover more JS

[Localization](https://github.com/stefalda/ReactNativeLocalization) library

[Many](https://github.com/react-native-community/react-native-elements) [native](https://github.com/GeekyAnts/NativeBase) components