---
layout: post
title: "Getting Started With SwiftUI"
date: 2020-02-14T10:49:33+08:00
categories: [SwiftUI]
---

I am getting started with SwiftUI, and there's so much to learn, all over again.

Luckily, there are plenty of articles and videos.

Here are some _random stuff_ that I learnt in the first few days as I try to build a new app. The last section will have more links to resources, and kind of cheat sheet.

I might just add more to this post as I learn, or might write new blog post specifically on some tricks even around the most common of things..

## Let's watch how to build Instagram feed

Watch on YouTube [part 1](https://www.youtube.com/watch?v=APxrtnxRzwI) & [part 2](https://www.youtube.com/watch?v=BiNYCvL1m94) where DeLong interactively teach SwiftUI while he replicates Instagram UI.

There are some framework designs that sucks eg. he doesn't like `NavigationView` and how nav links work. A hideous design about nav link is that all the destination views will be initialized!

## NavigationLink not working?

It is so common to not get nav link working. It seems like they are designed to be used only in `List`, or nav bar. You might need to use the `isActive` [trick](https://stackoverflow.com/a/59933501/242682) if the nav link is in a `VStack` or etc.

## How to push a view without using NavigationLink

A workaround is to use a [`hidden` nav link](https://stackoverflow.com/a/57321795/242682).

## [Present a modal sheet](https://www.hackingwithswift.com/quick-start/swiftui/how-to-present-a-new-view-using-sheets)

You need to 1. declare `@State isPresented = false`, 2. passing it to [`.sheet()`](https://developer.apple.com/documentation/swiftui/view/3352791-sheet), and 3. button action to set `isPresented = true`. Sorry it is long and ugly.

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

## View Extension

Custom modifier is a good way to refactor your code. Another way is by extending `View`.

```swift
extension View {
    public func customModifier(color: Color = .green) -> some View {
        return multilineTextAlignment(.center)  // Modifier 1
        .padding(.all)                    // Modifier 2
        .foregroundColor(color)          // Modifier 3, with `color` passed in
    }
}
```

You can even do [conditional modifier](https://swiftui-lab.com/view-extensions-for-better-code-readability/) (advanced).

When do you use `ViewModifier` and when do you use `View` extension? `ViewModifier` is more powerful because it can have instance variables and `@State`, so use it if that's needed.

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

## Sample PreviewProvider

```swift
Group {
    MyView()
    MyView()
        .environment(\.colorScheme, .dark)
        .environment(\.sizeCategory, .extraExtraExtraLarge)
        .previewDevice("iPhone 8") // Or .fixed
}
```

The `Group` displays 2 devices. You can have more, and configure as desired.

## Refactor the views

Because of the way it is designed, a complex view would have deeply nested code, with lots of modifier. To refactor relentlessly, a shortcut is to `Cmd+Click` on a view > **Extract subview**.

## Semantic Colors

Some examples:

```swift
.foregroundColor(Color.accentColor)
.foregroundColor(Color.pink)
.foregroundColor(Color(.label)) // Notice this uses an initializer
.foregroundColor(Color(.secondaryLabel))
.background(Color(.systemBackground))
```

## More Resources

- [https://fuckingswiftui.com/](https://fuckingswiftui.com/)
- [https://github.com/vlondon/awesome-swiftui](https://github.com/vlondon/awesome-swiftui)
- [https://github.com/ivanvorobei/awesome-ios-ui](https://github.com/ivanvorobei/awesome-ios-ui)
- [https://www.hackingwithswift.com/quick-start/swiftui/swiftui-tips-and-tricks](https://www.hackingwithswift.com/quick-start/swiftui/swiftui-tips-and-tricks)
