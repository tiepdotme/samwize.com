---
layout: post
title: "Fix npm/bower install with su"
date: 2015-09-07 18:32:23 +0800
comments: true
categories: [Node]
---

You should not be using `su` with `npm install` or `bower install`.

However, we understand when you see errors such as **npm ERR! Error: EACCES, mkdir ..**, you will relent to using your superadmin power..

<!-- more -->

In bower case, if you do, you will see warning message:

> Since bower is a user command, there is no need to execute it with superuser permissions.
> If you're having permission errors when using bower without sudo, please spend a few minutes learning more about how your system should work and make any necessary repairs.

The [fix](https://github.com/npm/npm/issues/5649) is simple:

    sudo chown -R $USER:$GROUP ~/.npm
    sudo chown -R $USER:$GROUP ~/.config

This makes the directory where npm and bower is using when installing packages to be given the correct permissions.