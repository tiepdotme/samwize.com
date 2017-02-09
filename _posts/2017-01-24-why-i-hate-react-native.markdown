---
layout: post
title: "Why I Hate React Native"
date: 2017-01-24T17:27:17+08:00
categories: [React Native]
---

I have spent 2 weeks using React Native for an **existing iOS app**, building a new feature that could be summed up as - providing a list of images which can be tapped on. 

It is an easy task that I could do in a day with the beautiful Swift and familiar `UIKit` framework. But things gets hairy with React Native..

React Native is changing rapidly, so what I mention here could only be relevant as of Jan 2017.


## What I Like

Let's start with what I like because that will be short.

My first impression with it is awesome. It is refreshing. Going through the Hello World example and more is enlightening.

Then there is code push. No more waiting for app review. 

So I went on to write a [basic guide](http://samwize.com/2017/01/09/react-native-for-ios-swift-developer/) and about [CodePush](http://samwize.com/2017/01/19/guide-to-integrating-codepush-for-ios-react-native-project/).


## Mixing React Native with True Native

By True Native, I am referring to all the frameworks provided by iOS: `UIKit`, `Foundation`, etc

It could be just me, because I have an existing code base that uses True Native. If you start a project from scratch using pure React Native, most likely you will not have the problem.

But if you are like me, you have to deal with extra problems.

The first is communication between the 2 platforms. [Quoted](http://facebook.github.io/react-native/docs/communication-ios.html):

> But, when we mix React Native and native components, we need some special, cross-language mechanisms that would allow us to pass information between them.

And this cross-language (or cross-platform) mechanism is very messy.

They provided a few ways, but none is beautiful.

You will get your hands dirty with **Objective-C** because React Native require macro to expose a module or function.


## Poor Documentation

It is written poorly, and lacking for some classes such as emitters.


## Deprecated, really?

I encountered this [issue](https://github.com/facebook/react-native/issues/8714) where they deprecate a method `sendAppEventWithName`, and recommended to use a subclass of `RCTEventEmitter`.

Yet, their [documentation](http://facebook.github.io/react-native/docs/native-modules-ios.html#sending-events-to-javascript) is still using the deprecated method.

Being a good developer, I use the new way. But alas! The new way didn't work. It could be either one the reason: I do not know how to use because there is no documentation, or it has a bug.

I believe there is a bug.


## Buggy

There are lots of bugs.

I filed an [issue](https://github.com/facebook/react-native/issues/12049) that is to do with a `Touchable` being fired when it's parent (a `UIScrollView`) is being scrolled.

Somehow I believe my issue will be of a lesser priority because I am mixing a true native `UIScrollView` with react native component.


## Time Wasted

As an early-user of a piece of programming platform, you will use much of your time to help the community. Good if you have the time. 

Otherwise, you will be wasting precious time. 
