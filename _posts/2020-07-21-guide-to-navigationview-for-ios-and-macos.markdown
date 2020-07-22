---
layout: post
title: "NavigationView for iOS and macOS"
date: 2020-07-21T18:02:18+08:00
categories: [SwiftUI]
---

`NavigationView` is the SwiftUI component to creating the UIKit's _equivalent_ of `UINavigationController` and `UI/NSSplitViewController`.

While it is possible to share code between iOS and macOS, this post will show how to do for each platform individually, for clarity.

There are also 2 types to know:

1. Standard Navigation: push and pop like `UINavigationController`
2. Split view/columns

## iOS: Standard Navigation

This is simple, using the `StackNavigationViewStyle()`.

```swift
var body: some View {
    NavigationView {
        // This is the root view
        NavigationLink(destination: Text("Pushed view")) {
            Text("Push")
        }
    }
    .navigationViewStyle(StackNavigationViewStyle())
}
```

# iOS: Split view

[NavigationView](https://developer.apple.com/documentation/swiftui/navigationview) inits with a `ViewBuilder` where the first view is the first panel, usually the sidebar. The subsequent views are the placeholders for the other panels.

The navigation view uses `DoubleColumnNavigationViewStyle()`, which is a split view with 2 columns. But you can do similarly for 3 columns.

```swift
struct SplitView: View {
    var body: some View {
        NavigationView {
            panel1 // View 1

            Text("Placeholder for Panel 2") // View 2
        }
        .navigationViewStyle(DoubleColumnNavigationViewStyle())
    }

    var panel1: some View {
        VStack {
            Text("Panel 1")
            NavigationLink(destination: panel2) { // Change panel 2
                Text("Go to Panel 2")
            }
        }
    }

    var panel2: some View {
        Text("Actual Panel 2")
    }
}
```

## macOS

The biggest difference with macOS is that:

1. You need to provide the sizes for the panels, using `frame`
2. There's no `StackNavigationViewStyle()`, so only column style is available

```swift
struct SplitView: View {
    var body: some View {
        NavigationView {
            panel1

            Text("Placeholder for Panel 2")
            .frame(maxWidth: .infinity, maxHeight: .infinity) // Fill content view
        }
        // MUST provide the "window size"
        .frame(minWidth: 800, maxWidth: .infinity, minHeight: 500, maxHeight: .infinity)
    }

    var panel1: some View {
        VStack {
            Text("Panel 1")
            NavigationLink(destination: panel2) {
                Text("Go to Panel 2")
            }
        }
        .frame(minWidth: 100, idealWidth: 150, maxWidth: 200, maxHeight: .infinity) // Sidebar width
    }

    var panel2: some View {
        Text("Panel 2")
        .frame(maxWidth: .infinity, maxHeight: .infinity) // Fill content view
    }
}
```

A typical layout is to have a smaller sidebar on the left, and a content view that fills the rest of the window.

Again, `NavigationView` can manage multiple columns.
