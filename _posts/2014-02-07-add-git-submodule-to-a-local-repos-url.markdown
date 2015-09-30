---
layout: post
title: "Add git submodule to a local repos url"
date: 2014-02-07 00:36:20 +0800
comments: true
categories: Git
---

I was adding a [submodule](http://git-scm.com/docs/git-submodule) which is local.

I didn't push to a remote because it is not necessary for my project. It is just some data files that I wanted to track the changes locally. Yet, I require it in the main project repository.

A supposedly simple operation, but I met with some problem.

<!-- more -->

## Problem

I use [sourcetree](http://www.sourcetreeapp.com) app to add the submodule. On my first try, somehow I select the folders wrongly.

After a few tries, I figured out the source path and relative path in sourcetree..

But when I add, I keep getting the error:

    pathspec 'mysub' did not match any file(s) known to git


## Switch to command line

Whenever sourcetree fails me, I switch to the command line.

I deleted `.gitmodules` and the `mysub` directory (the relative directory in my main project that I want my submodule to be in).

Then I run:

    git submodule add /path/to/submodule mysub

It complained:

    A git directory for 'mysub' is found locally with remote(s):
    origin  /path/to/submodule

I used the `--force` options, and it gave me:

    fatal: You are on a branch yet to be born


## Solution

After wasting 20 minutes, I found a [solution on StackOverlfow](http://stackoverflow.com/a/12072517/242682) with more than 100 upvotes.

I have to delete `.git/modules/mysub` because when I was using sourcetree, on my first try, the URL/path was specified wrongly.

A silly mistake.

But looks like I am not the only one.
