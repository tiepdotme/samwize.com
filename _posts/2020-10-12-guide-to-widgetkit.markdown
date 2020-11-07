---
layout: post
title: "Guide to WidgetKit"
date: 2020-10-12T14:17:20+08:00
categories: [WidgetKit]
---

## 1. Create widget extension

Create a new target and select Widget Extension. An extension is like a mini-app, with it's own "application identifier".

In our example, we name it "MyWidget". A corresponding "MyWidgetExtension.appex" will be automatically embedded in your app target.

## 2. fastlane match

If you're using fastlane, then you don't want Xcode to manage the signing. In that case, there is an extra step to create a new set of application identifier for the extension.

    fastlane produce -a com.just2us.myapp.mywidget --skip_itc

Then run `fastlane match` for the new identifier to set up and download the provisioning profiles. Then in Xcode, disable "Automatically manage signing" and select the provisioning profile.

## 3. Configure widget

When you create a widget, the initial "Hello World" code will all be in 1 file. I will immediately break it into 3 files, because there are multiple components at work.

First of all, the `Widget` protocol let us describe the widget. It is the extension entry point.

```swift
@main
struct MyWidget: Widget {
    let kind: String = "MyWidget"

    var body: some WidgetConfiguration {
        StaticConfiguration(kind: kind, provider: Provider()) { entry in
            ShortcutsView(entry: entry)
        }
        .configurationDisplayName("My Widget")
        .description("A shortcut widget for the app.")
        .supportedFamilies([.systemSmall])
    }
}
```

Our example is a "shortcut" for the app, and supports only the small (square) size.

We use the simple `StaticConfiguration`. We will not dive into the dynamic intent-based configuration in this post.

The widget also states 2 of our custom classes (which you'll see later):

1. `Provider` - our timeline provider
2. `ShortcutsView` - our SwiftUI view to render

## 4. Timeline Provider

This is how a widget works: iOS will ask your widget for a [timeline](https://developer.apple.com/documentation/widgetkit/keeping-a-widget-up-to-date) so that iOS can update your widget at the right time.

To do that, you have to provide your implementations for `TimelineProvider`, and your `TimelineEntry` struct.

A minimal `TimelineEntry` requires a `date` field. Obviously, the date tells iOS the datetime to update the widget.

```swift
struct SimpleEntry: TimelineEntry {
    let date: Date
}
```

`TimelineProvider` ask for 3 things:

```swift
struct Provider: TimelineProvider {
    func placeholder(in context: Context) -> SimpleEntry {
        SimpleEntry(date: Date())
    }

    func getSnapshot(in context: Context, completion: @escaping (SimpleEntry) -> ()) {
        let entry = SimpleEntry(date: Date())
        completion(entry)
    }

    func getTimeline(in context: Context, completion: @escaping (Timeline<SimpleEntry>) -> ()) {
        let timeline = Timeline(entries: [SimpleEntry(date: Date())], policy: .atEnd)
        completion(timeline)
    }
}
```

## 5. The SwiftUI View

This is where your widget creates the view.

```swift
struct ShortcutsView: View {
    @Environment(\.widgetFamily) var family: WidgetFamily
    var entry: Provider.Entry

    var body: some View {
        ...
    }
}

struct ShortcutsView_Previews: PreviewProvider {
    static var previews: some View {
        ShortcutsView(entry: SimpleEntry(date: Date()))
            .previewContext(WidgetPreviewContext(family: .systemSmall))
    }
}
```

You will likely want to use the `widgetFamily` environment key to create the view according to the size.

That's all for the basics of creating a widget.

---

## Open shortcut link

When user taps on a widget, the host app will open. You can also specify a link to open. There are 2 ways to provide the link (be it deeplink or universal link):

1. `widgetURL` modifier
2. `Link` view

The difference is that `widgetURL` is for the whole widget view; therefore only 1 for a widget.

So most likely you will use the [SwiftUI Link](https://developer.apple.com/documentation/swiftui/link) for different views in a widget eg. news widget.

Handle the URL in AppDelegate (even if you have [already handled in your SceneDelegate](/2020/10/09/how-to-handle-deeplink-for-uiscene/)).

```swift
func application(_ app: UIApplication, open url: URL, options: [UIApplication.OpenURLOptionsKey : Any] = [:]) -> Bool {
    // Handle the url
    ...
}
```

## Small size does NOT support Link

For the small squarish widget, you cannot have multiple links in it. In fact you cannot even have 1 link. Instead, you have to use `widgetURL`.

This seems to be a design enforcement.

## UI Guide & radius

The [Human Interface Guideline](https://developer.apple.com/design/human-interface-guidelines/ios/system-capabilities/widgets) details how we should design good glanceable widgets.

A new API to help with corner radius is [ContainerRelativeShape](https://developer.apple.com/documentation/swiftui/containerrelativeshape). It is a shape, like `RoundedRectangle`, but the corner radius adapt to it's parent. With that, you need not hard code any corner radius in the widget.

The standard margin is 16pt.

## Multiple Widgets

You can have different types of widgets in an app. If your app support more than 1 widget, then you would use the `WidgetBundle` instead.

```swift
@main
struct MultipleWidgets: WidgetBundle {
    @WidgetBundleBuilder
    var body: some Widget {
        Widget1()
        Widget2()
    }
}
```

## PITFALL: Conflicting provisioning settings

> XyzWidgetExtension has conflicting provisioning settings. XyzWidgetExtension is automatically signed for development, but a conflicting code signing identity iPhone Distribution has been manually specified...

If signing has the above error, it could likely just be Xcode generating improperly. Uncheck "Automatically manage signing", then check it back, and select a Team.

Another possible error.

> Embedded binary is not signed with the same certificate as the parent app. Verify the embedded binary target's code sign settings match the parent app's.

For that, make sure the app and extension use the same signing cert. If using fastlane match, read the section above to set up signing.

## PITFALL: App review rejection

A rejection story. When I release a widget for [Torchlight app](https://torchlight.app), it was rejected.

![](/images/apple-app-review-call_ticket_and_my_reply.jpg){:.border}

> Specifically, your app’s widget only provides users shortcut to your app’s features, which is not appropriate.

I had a call with an Apple app reviewer, and was told that if I appeal, it will "100% be rejected". I continue to appeal, quoting other apps that were shortcuts too, and that my torchlight widget is similar to iOS torch shortcut (which is in lockscreen)!

A week later, it was approved! So looks like nothing is 100% when it comes to app review.
