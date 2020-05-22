---
layout: post
title: "Wrapping UIView in SwiftUI"
date: 2020-05-22T15:53:04+08:00
categories: [SwiftUI]
---

When I started out SwiftUI, I thought I will not need to go back to UIKit stuff. But I am wrong, for 2 reasons:

1. SwiftUI is very incomplete
2. SwiftUI is very buggy

SwiftUI is incomplete because it does not provide `WebView`, or `MailComposeView`, etc.

I can forgive for being incomplete since the framework is still in early days. But for being buggy, that didn't speak well of Apple, again.

My last straw is with their `TextField`. It does not work well for Chinese input. You can never type more than a few characters before it becomes _wonky_.

<iframe width="560" height="315" src="https://www.youtube.com/embed/0X24Z1S6qbM" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

I believe we gonna depend on UIKit's components, for a long long time.

## Wrapping UITextField

```swift
struct WrappedTextField: UIViewRepresentable {
    func makeUIView(context: Context) -> UITextField {
        let textField = UITextField()
        textField.delegate = context.coordinator
        return textField
    }

    func updateUIView(_ uiView: UITextField, context: Context) {
    }

    func makeCoordinator() -> Coordinator {
        return Coordinator()
    }

    class Coordinator: NSObject, UITextFieldDelegate {
    }
}
```

The code is made up of:

1. 3 methods to implement SwiftUI `UIViewRepresentable` protocol
2. A nested `Coordinator` for the often delegate pattern, which can be omitted if there's no delegate

That **bare minimum code** will solve the Chinese input bug, since it is the old and reliable `UITextField`. To make it useful, the wrapper needs to add a `Binding<String>` for the SwiftUI world.

The complete code to have a usable textfield is such:

```swift
struct WrappedTextField: UIViewRepresentable {
    @Binding var text: String // Declare a binding value

    func makeUIView(context: Context) -> UITextField {
        let textField = UITextField()
        textField.delegate = context.coordinator
        return textField
    }

    func updateUIView(_ uiView: UITextField, context: Context) {
        uiView.text = text // 1. Read the binded
    }

    func makeCoordinator() -> Coordinator {
        return Coordinator(text: $text)
    }

    class Coordinator: NSObject, UITextFieldDelegate {
        @Binding var text: String

        init(text: Binding<String>) {
            self._text = text
        }

        func textFieldDidChangeSelection(_ textField: UITextField) {
            DispatchQueue.main.async {
                self.text = textField.text ?? "" // 2. Write to the binded
            }
        }
    }
}
```

The 2-way binding is provided by

1. `updateUIView` to read the binded text
2. In Coordinator, the delegate method `textFieldDidChangeSelection` will write to the binded text. Note that it is wrapped with a main queue dispatch because if not, there will be a warning:

    > Modifying state during view update, this will cause undefined behavior

## An Xcode Template

Because much of the boilerplate code follows a strict pattern, I create a [code snippet](/2014/03/26/tip-use-xcode-snippets/) so that I can wrap `UIView` easily, and then focus my time on configuring the actual view.

```swift
struct <#WrappedUIView#>: UIViewRepresentable {
    @Binding var text: String

    func makeUIView(context: Context) -> <#UIView#> {
        let view = <#UIView#>()
        view.delegate = context.coordinator
        return view
    }

    func updateUIView(_ uiView: <#UIView#>, context: Context) {
        uiView.text = text
    }

    func makeCoordinator() -> Coordinator {
        return Coordinator(text: $text)
    }

    class Coordinator: NSObject, <#UIViewDelegate#> {
        @Binding var text: String

        init(text: Binding<String>) {
            self._text = text
        }

        func someDelegateMethod(_ uiView: UIView) {
            DispatchQueue.main.async {
                self.text = uiView.text ?? ""
            }
        }
    }
}
```

The code still has to be edited for the specific `UIView` purpose eg. instead of `Binding<String>` it could be other types, and the delegate methods.

## Wrapping UIViewController

It is very similar for wrapping a `UIViewController`. In all the code that has a "-UIView", replace it with "-UIViewController" 🤓
