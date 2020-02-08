---
layout: post
title: "Bet You Didn't Know: You Don't Need 5.5 Inch Screenshots for App Store"
date: 2020-02-08T11:48:21+08:00
categories: [App Store, Rejections]
---

The [App Store screenshots specification](https://help.apple.com/app-store-connect/#/devd274dd925) (as of Feb 2020) states that you need **at least these two sizes**:

1. 6.5 inch (iPhone 11 etc)
2. 5.5 inch (iPhone 8 etc)

After a series of app review rejections, I learnt that **reviewers can approve your app without 5.5" screenshots**!

Take a look of my app in developer portal:

![](/images/dualgram-no-5-5.jpg){:.border}

## Does it mean the app does not support 5.5"?

Absolutely NOT.

If you check out my app on a 5.5" device, it will still show up, but the screenshots is using 6.5" ones..

![Without 5.5" vs With 5.5", on iPhone 8](/images/dualgram-on-5_5-device.jpg){:.border}

So, it seems App Store will **make 5.5" screenshots optional** in the future. For now, app reviewers are given the power to approve without 5.5".

## Why my app can do without 5.5"?

Let me now tell the whole story, which doesn't end well.

My app -- [Dualgram](https://dualgram.com) -- doesn't support 5.5 inch devices (iPhone 8 and below). So the screenshot warns users NOT to download (above screenshot, on the right).

The reviewer rejects on the grounds of **Accurate Metadata**.

> We noticed that your screenshots do not sufficiently reflect your app in use.

I appealed, but failed.

_(Strangely, 2 months ago, I appealed for the same issue, and succeeded.)_

Then this suggestion:

![](/images/dualgram-rejected-remove-5_5-2.jpg){:.border}

I tried, but couldn't submit, so I ask for help:

![](/images/dualgram-rejected-remove-5_5-3.jpg){:.border}

Hail to their power, they override the error, and approved!

## Not what I expected

I thought that without 5.5" screenshot, the app will not support 5.5" devices.

But no, it simply uses the 6.5" screenshots..

No good at all, for Dualgram..

_Related:_

- [I'm Not Surprise With App Store Review Rejections Anymore](/2019/09/20/i-am-no-longer-surprise-over-app-store-review-rejections/)
