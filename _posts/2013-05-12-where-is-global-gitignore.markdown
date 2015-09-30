---
layout: post
title: "Where is global gitignore?"
date: 2013-05-12 14:59
comments: true
categories: [Git]
---

I made a commit to a git repos and broke the build.

(I should be punished to clean the toilet for a week for accidents like this)

But what happened was unexpected, because I added a framework (Crashlytics), and I thought the library will be pushed fine to the repos.

What I didn't expect was that my GLOBAL gitignore has an entry for Crashlytics framework.

For developers who didn't know, other than the project gitignore file, there is also a global file for your machine.

It is at `~/.gitignore_global`.