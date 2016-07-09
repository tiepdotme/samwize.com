---
layout: post
title: "CloudKit Development Guide"
date: 2016-07-07T17:37:02+08:00
categories: []
---

## iCloud Security

Apple provided a [security and privacy overview](https://support.apple.com/en-sg/HT202303).

In short, all data at transit are encryted. All data at rest on server are encrypted.

A minimum of 128-bit AES encryption is used.

Moreover, iCloud keychain encryption keys are created on your own devices and Apple can't access them. Apple cannot access any of the keys that could be used to decrypt your data, and only trusted devices that you have approved can access your iCloud keychain.

