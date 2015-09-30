---
layout: post
title: "Relative path when using require and fs for node.js"
date: 2014-02-05 08:40:24 +0800
comments: true
categories: Node
---

When using `require`, the path is relative to that source file (NOT root directory).

When using `fs`, the path is relative to `process.cwd()` (NOT that source file).

The relativity is [different](https://github.com/joyent/node/issues/1326), yet should be a [good design](http://stackoverflow.com/a/10861719) from the node developers. If you always require a certain file, perhaps put it in node_modules.
