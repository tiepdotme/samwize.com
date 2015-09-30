---
layout: post
title: "Multiple Heroku Account"
date: 2014-03-07 14:38:04 +0800
comments: true
categories: [Heroku]
---

I encountered a problem with Heroku when I started using multiple accounts on the same machine.

This is when this [accounts plugin](https://github.com/ddollar/heroku-accounts) helps.

<!-- more -->

When I tried to push to my repos, I got the following error:

     !  Your account someone@gmail.com does not have access to myapp.
     !
     !  SSH Key Fingerprint: xx:xx:xx

Doing a `heroku login` with the account didn't help to 'logout the previous'. The [accounts plugin](https://github.com/ddollar/heroku-accounts) solved the problem.

The plugin is also useful as it helps to set up ssh fingerprint in 1 command!
