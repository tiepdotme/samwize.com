---
layout: post
title: "What is the replacement for iOS UDID?"
date: 2013-04-26 00:11
comments: true
categories: [iOS]
---

Apple has long announced `uniqueIdentifier` UDIDs would be deprecated. From 1st May 2013, Apple will reject apps that use the deprecated identifier.

So what you have do?

DoubleEncore has writtern a good article on the [other ways to generate UDID](http://www.doubleencore.com/2013/04/unique-identifiers/). It is up to you and your use case.

For me, [OpenUDID](https://github.com/ylechelle/OpenUDID) is the nearest replacement. The only difference is that it cannot persist the identifier if there is a system reset.