---
layout: post
title: "Tip: Use Xcode Snippets"
date: 2014-03-26 20:26:20 +0800
comments: true
categories: [iOS]
---

This isn't a big secret at all, yet many developers are not utilising the code snippet feature. Myself included.

But I am now making a habit of using my code snippet library, which is conveniently synced to my [Dropbox](https://db.tt/GRqFn03).

> Remember, programming isn't about being an expert in typing or memorizing code. So help yourself to code snippets!

<!-- more -->

## Snippets in Xcode

If you don't know how to use snippets in Xcode, read this [guide from NSHipster](http://nshipster.com/xcode-snippets/).

A summary of adding your own snippet:

1. Drag your code to the Xcode snippet library (at bottom right, with a `{ }` icon)

2. Enter description and completion shortcut if you want

3. To use placeholder text, use `<#your-placeholder#>`.


## Sync your Snippets

All snippets are stored the Xcode directory.

You can create symoblic link to your Dropbox, therefore back them up.

    ln -s ~/Library/Developer/Xcode/UserData/CodeSnippets ~/Dropbox/Workspace/Xcode

I store them under `~/Dropbox/Workspace/Xcode`. It's up to you where to store. [Signup for Dropbox](https://db.tt/GRqFn03) if you don't have an account.

PS: I would attempt to store them in Github, someday.


## Sync to multiple machines

You can then sync to multiple machines, or restore them on a new machine!

    ln -s ~/Dropbox/Workspace/Xcode/CodeSnippets ~/Library/Developer/Xcode/UserData/CodeSnippets


## Snippets.me App

[Snippets.me](http://snippets.me) is an app for Mac and Windows that store snippets for all kinds of languages.

I don't recommend to use the app, especially if you are just using Xcode.

It is not as convenient to use, and it [doesn't provide syncing](http://blog.snippets.me/2013/09/using-dropbox-icloud-sync-in-snippets.html). And if it does, it will be paid. Better off using Xcode.

