---
layout: post
title: "The Rules for using External Storage in Core Data"
date: 2014-07-04 16:12:26 +0800
comments: true
categories: iOS
---

Core Data is able to store binary data (NSData or UIImage) for an attribute.

And you can enable "Allows External Storage" to let Core Data manage the binary data eg. move it to external file if it hits a certain threshold.

However, a [good rule of thumb](http://stackoverflow.com/a/2098401/242682) as follows:

<!-- more -->

- < 100 kb store in the related entity (person, address, whatever).
- < 1 mb store in a separate entity on the other end of a relationship to avoid performance issues.
- \> 1 mb store on disk and reference the path in your Core Data store.

Therefore, I will usually store thumbnail and a relationship to the original in the entity.

For example:

Person.photoThumbnail (100 kb)

Person.photo is a relationship to Image.image (1 mb)
