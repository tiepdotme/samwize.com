---
layout: post
title: "SwiftUI and Data Flow"
date: 2020-06-29T15:01:38+08:00
categories: [SwiftUI]
---

WWDC 2020 has clarify much on how we should manage data flow when using SwiftUI.

I had a hard time understanding the role of `StateObject`, until I watched [Session 10040: Data Essentials in SwiftUI](https://developer.apple.com/videos/play/wwdc2020/10040).

[![](/images/source-of-truth-timeline.jpg)](https://developer.apple.com/videos/play/wwdc2020/10040)

The [Apple documentation](https://developer.apple.com/documentation/swiftui/managing-model-data-in-your-app) was also helped.

## The right way to use

It is very important to know how to use the various components correctly.

I have been writing in SwfitUI 1.0, and now I know I have bugs. I have misused `@ObservedObject`.

Here are a few things to remind myself.

> SwiftUI might create or recreate a view at any time,

`View` is a descriptor of the view, not like typical `UIView`. They are lightweight. It is unwise to override `View.init`. If you have _anything_ to do with states and data objects, you do it using one of the state components provided.

The state is **attached** to the view by SwiftUI.

App Hierarchy: `App` -> `Scene` -> `View`

The most important decision is to decide **where to store the truth**.

_Is it a global truth for the app, or across scenes, or is it for an individual view?_

## StateObject vs ObservedObject

One of the most common [question](https://stackoverflow.com/q/62544115/242682) is how to use these 2 correctly.

We initialise with `@StateObject` when we want the view/scene/app to hold on to the truth.

```swift
struct LibraryView: View {
    @StateObject var book = Book() // Hold on to the 1 truth
    var body: some View {
        BookView(book: book) // Pass it to another view
    }
}
```

We declare `@ObservedObject`, but never initialize it. When declared in a view, it becomes the view's dependency. Some view higher up will have to pass to it. In this case, `LibraryView` pass the source of truth to `BookView`.

```swift
struct BookView: View {
    @ObservedObject var book: Book // From external source
}
```
