---
layout: post
title: "How to Support the Export of a Certain Content Type Eg. Photo, Audio"
date: 2020-02-20T18:06:14+08:00
categories: []
published: false
---

[App Extensions](https://developer.apple.com/library/archive/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214-CH20-SW1) provide multiple ways for your app to handle other content type, even originating from other apps.

## 1. Adding Document Type and UTI

To make an app handle "Open in ..." menu, you have to [specify the document type it supports](https://developer.apple.com/library/archive/qa/qa1587/_index.html).

1. Target > Info > Add Document Type
2. Target > Info > Add custom Exported UTIs (if needed)

These are the [public UTIs](https://developer.apple.com/library/archive/documentation/FileManagement/Conceptual/understanding_utis/understand_utis_conc/understand_utis_conc.html#//apple_ref/doc/uid/TP40001319-CH202-CHDHIJDE). You only need to add custom UTIs if they are not those public standard ones.

When you export/share a file, in the available apps will have yours eg. "Copy to YourApp".

[If](https://stackoverflow.com/q/33517345/242682) you want just "YourApp", you need to implement share extension.

You also need to [implement how you can handle the document](https://developer.apple.com/library/archive/documentation/General/Conceptual/ExtensibilityPG/FileProvider.html#//apple_ref/doc/uid/TP40014214-CH18-SW1).

## 2. Action

An action extension is unlike a handling a full fledged document.

In iOS, the action is usually a view (modal or action sheet), helping the host app to view, or even edit, the data in a different way. 

> In iOS, an Action extension is listed in the action area of the activity view controller that appears when users tap the Share button.
