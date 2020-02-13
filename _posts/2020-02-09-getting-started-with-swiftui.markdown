---
layout: post
title: "Getting Started With SwiftUI"
date: 2020-02-09T22:49:33+08:00
categories: [SwiftUI]
---

I am getting started with SwiftUI, and there's so much to learn, all over again.

Luckily, there are plenty of articles and videos.

Here's some that I learnt from, and the last section with links to resources.

## Let's watch how to build Instagram feed

Watch on YouTube [part 1](https://www.youtube.com/watch?v=APxrtnxRzwI) & [part 2](https://www.youtube.com/watch?v=BiNYCvL1m94) where DeLong interactively teach SwiftUI as he replicates Instagram UI.

There are some framework designs that sucks eg. he doesn't like `NavigationView` and how nav links work.

## How to push a view without using `NavigationLink`

A workaround is to use a [`hidden` nav link](https://stackoverflow.com/a/57321795/242682).

## [Present a modal sheet](https://www.hackingwithswift.com/quick-start/swiftui/how-to-present-a-new-view-using-sheets)

You need 1. declare `@State isPresented = false`, 2. passing it to [`.sheet()`](https://developer.apple.com/documentation/swiftui/view/3352791-sheet), and 3. button action to set `isPresented = true`.

It is long and ugly.

## [2 ways to dismiss (eg. sheet)](https://www.hackingwithswift.com/quick-start/swiftui/how-to-make-a-view-dismiss-itself)

Use environment, or pass a binding to the sheet's `isPresented`.

## How to observe an `@State` or a control?

You will often have a `@State` and pass it to a control such as `Toggle` or `TextField`. But you can't just observe the state with `didSet`. Instead, you need to create a binding like [this](https://stackoverflow.com/a/59040171/242682).

## Modifiers

The concept is used throughout, applying modifier after modifier repeatedly. It is using **Builder design pattern**.

```swift
SomeView(...)
    .multilineTextAlignment(.center)  // Modifier 1
    .padding(.all)                    // Modifier 2
    .foregroundColor(.green)          // Modifier 3
```

It tends to get messy and with many lines. A trick here is to create customer modifiers.

## Custom Modifiers

```swift
struct CustomModifier: ViewModifier {
    func body(content: Content) -> some View {
        content
          .multilineTextAlignment(.center)  // Modifier 1
          .padding(.all)                    // Modifier 2
          .foregroundColor(.green)          // Modifier 3
    }
}
```

With that, we can now use it on any View by applying 1 `modifier`.

```swift
SomeView(...)
  .modifier(CustomModifier())
```

## Applying modifiers to a group of views

`Group` is a logical grouping, without any layout specific layout like `HStack` or `Form`. When you apply a modifier to a group, it will apply to every view in the group.

## View Erasure

Because SwiftUI need to know the view type during compile time, there will be times you need to erase the type eg. to make 2 views they same type. `AnyView` will do that.

## Alert with multiple custom buttons

`Alert` now supports only 2 buttons, and will possibly make it more flexible. Alternatively, [ActionSheet](https://developer.apple.com/documentation/swiftui/actionsheet).

## Animation & Transition

Add `animation()` to a binding, or use `withAnimation()` with state, to animate the view changes.

The default transition is fade. To change, use the `transition()` modifier.

## Resume the live preview

Because Xcode will pause the live preview when there is significant update, it is useful to know the shortcut to resume `Option+Cmd+P`.

## Sample `PreviewProvider`

```swift
Group {
    MyView()
    MyView()
        .environment(\.colorScheme, .dark)
        .environment(\.sizeCategory, .extraExtraExtraLarge)
        .previewDevice("iPhone 8") // Or .fixed
}
```

The `Group` shows 2 devices. You can have more, and configure as desired.

## Refactor the views

Because of the way it is designed, a complex view would have deeply nested code, with lots of modifier. To refactor relentlessly, a shortcut is to `Cmd+Click` on a view > **Extract subview**.

## More Resources

- https://fuckingswiftui.com/
- https://github.com/vlondon/awesome-swiftui
- https://github.com/ivanvorobei/awesome-ios-ui
- https://www.hackingwithswift.com/quick-start/swiftui/swiftui-tips-and-tricks
