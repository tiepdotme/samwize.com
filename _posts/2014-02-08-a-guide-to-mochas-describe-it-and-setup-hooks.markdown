---
layout: post
title: "A guide to mocha's describe(), it() and setup hooks"
date: 2014-02-08 02:26:43 +0800
comments: true
categories: Node
---

[Mocha](http://samwize.com/2014/01/29/mocha-the-most-popular-testing-framework-for-node/) is a wonderful testing framework for node.js, however the [documentation](http://visionmedia.github.io/mocha/) seems to be lacking.

When I first begin to write in Mocha, I had many questions:

- what exactly does `describe()` do?
- what is the effect of nesting `describe()` multiple levels?
- is `describe()` a test suite, and `it()` a test case?
- where should I place `before()` and `after()` hooks?

<!-- more -->

I didn't find the answer online, hence I simply _test it out myself_.

This [gist](https://gist.github.com/samwize/8877226) is the javascript that explains the questions I had. Much clearer now :)

I also used [docco](http://samwize.com/2014/01/31/the-best-documentation-generator-for-node/) to generate a more-pleasing-to-read documentation for the gist.

Sorry but I didn't host the docco documentation, but you could still read the javascript in it's purest form:

{% gist 8877226 %}
