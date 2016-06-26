---
layout: post
title: "Download all WWDC 2014 PDF Slides only"
date: 2014-06-06 00:56:46 +0800
comments: true
categories: [WWDC]
---

[WWDC 2014](https://developer.apple.com/videos/wwdc/2014/) is over, and all the videos and PDF slides are now available for developers to download.

There is a bash script going around on how to download all the HD videos.

But really, who has so much time to watch so many video?

<!-- more -->

Usually, I download all the slides, go through them, and choose which video is worth watching.

To download only the PDF slides, you can run this:

```bash
curl https://developer.apple.com/videos/wwdc/2014/ \
| grep -iIoh 'http.*pdf?dl=1' \
| sed -e 's/\?dl=1//g' \
| xargs -n1 curl --remote-name
```

This is a small problem with the last item grep-ed. I don't know how to fix. But you can download the last PDF with:

```bash
curl --remote-name http://devstreaming.apple.com/videos/wwdc/2014/102xxw2o82y78a4/102/102_platforms_state_of_the_union.pdf`
```

To download all the HD videos:

```bash
curl https://developer.apple.com/videos/wwdc/2014/ \
| grep -iIoh 'http.*._hd_.*dl=1">HD' \
| sed -e 's/\?dl=1">HD//g' \
| xargs -n1 curl --remote-name
```
