---
layout: post
title: "How to securely store secret keys in iOS app"
date: 2014-09-19 19:53:52 +0800
comments: true
categories: [iOS]
---

Strings in an iOS app can be easily extracted with `strings myapp`.

Taking a few more steps will make your app more secure.

Chris Hulbert has a good post on the topic to [obfuscate your secret keys](http://www.splinter.com.au/2014/09/16/storing-secret-keys/).

<!-- more -->

## A summary

Firstly, the technique is only good for secret keys that are not sent over the Internet. For example, secret keys that are used to encrypt a packet, sent over the Internet, then decrypt with the key on the server.

There are a few techniques of various levels:

- hex instead of literal strings

- obfuscate the hex with some hashing

- disable debug sessions

PS: This deters most of the hackers. But nothing is foolproof.
