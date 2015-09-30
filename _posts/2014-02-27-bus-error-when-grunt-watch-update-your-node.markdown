---
layout: post
title: "Bus error when grunt watch - Update your node"
date: 2014-02-27 14:45:52 +0800
comments: true
categories: [Node]
---

I was using grunt [watch](https://github.com/gruntjs/grunt-contrib-watch/) for [less](https://github.com/gruntjs/grunt-contrib-less), and encountred a kind of bus error:

    Running "watch" task
    Waiting...[1]    54136 bus error  grunt --stack

Turned out it is [due to](https://github.com/gruntjs/grunt-contrib-watch/issues/204) node version.

<!-- more -->

This is what you can do to update your node version to a latest stable version, using [n](https://github.com/visionmedia/n) (a node version management).

    sudo npm cache clean –f
    sudo npm install -g n
    sudo n stable

As of writing, `node -v` now gives me v0.10.26, and I can run grunt without the bus collision error.