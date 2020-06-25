---
layout: post
title: "Download All WWDC 2020 Videos"
image: /images/wwdc-2020-downloaded.jpg
date: 2020-06-25T11:25:25+08:00
categories: [WWDC]
---

In [2016](/2016/06/16/swift-script-to-download-all-wwdc-2016-videos-and-pdfs-automatically/), I wrote a [Swift script](https://github.com/samwize/wwdc-dl) to download all the WWDC PDFs and videos from the command line.

I have fixed for WWDC 2020. You can download the [binary](https://github.com/samwize/wwdc-dl/releases/tag/wwdc2020), then download all with:

    ./wwdc-dl -a

The script is still in a mess. Prime time for re-writing in Swift 5.3, with the [proper CLI support](https://swift.org/blog/argument-parser/). _But I am more likely to spend the time exploring SwiftUI 2.0, Catalyst, and awesome new features._ 😁

What's more, the official app will catch up eventually.

![All in one place](/images/wwdc-2020-downloaded.jpg)

## Apple Developer App

The [Apple Developer app](https://apps.apple.com/app/id640199958) is getting better. You can download videos, play them at 2x speed (unfortunately, there's the fastest), and there's even has a code section for easy copying!

![](/images/apple-developer-code-copy.jpg)

But there's still much room for improvement, as it is clearly a Catalyst app for iPhone.

![On my iMac 27"..](/images/apple-developer-app-on-imac.jpg)

I still prefer to download all and browse them in Finder, and playing at variable speed (1.75x, 3x, whatever) with VLC player. I can also take snapshots easily (especially since 2020 there's no PDF).

Features will eventually catch up. Just like Swift UI 2.0. Just last week I was still brooding over how to make certain layouts and features, but now it is all doable.
