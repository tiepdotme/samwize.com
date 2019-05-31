---
layout: post
title: "Mac Tips: Using Finder to Rename Multiple Files, Etc"
date: 2019-05-31T09:46:03+08:00
categories: [Mac]
---

I often need to rename multiple files eg. screenshots generated from UI Testing, or my personal photos. I often need to resize images eg. for screenshots on my blogs.

To do that easily, I often use my [handy bash commands](/2017/01/02/handy-bash-commands/). They are great, and I do have lots of control.

But macOS Mojave has features covering some of my very common workflow, which I didn't know till now.

## Rename Multiple Files with Finder

Select multiple files > right click to bring up the menu > **Rename XX items**.

And the renaming is powerful. There are 3 modes:

1. Replace Text
2. Add Text
3. Format

![Finder Rename Panel](/images/finder-rename-files.jpg)

The (3) Format is a nice one that prefix/suffix a running index, or date, to a name. That is very helpful especially for my photo management! I just wish the format can accept my own regex 😁

## Resize Image, Save as JPEG

When I write my blogs, it is often that I take screenshots (in PNG), and then I need to resize them to 600x600, and then convert to JPEG format.

In the past, I use [magick command](/2017/01/02/handy-bash-commands/), but there is a faster way using **Quick Action**.

I can highlight multiple files > right click > Services > Jpeg-600 (a custom Quick Action)!

![Services Menu](/images/quick-action-menu.jpg)

To create the custom Quick Action, you can open Automator.app and create it. The workflow looks like this:

![Quick Action](/images/automator-quick-action.jpg)

Further more, I have a Jpeg-1200 and Jpeg-800, and I create Keyboard shortcuts for them.

![Keyboard Shortcuts](/images/keyboard-shortcuts-services.jpg)
