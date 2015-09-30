---
layout: post
title: "Know the actual exact commands for an alias"
date: 2014-02-12 11:34:27 +0800
comments: true
categories: Mac
---

Since I use [Dotfiles](http://samwize.com/2014/01/12/getting-started-with-dotfiles/), which comes bundled with many alias, it is easy to lose track of the exact commands an alias refers to..

This morning, I typed `gl` on my zsh bash, thinking it is `git log`, but turned out it is `git pull`..

<!-- more -->

### 2 bash commands

`type` will resolve what the alias is for:

    $ type gl
    $ gl is an alias for git pull

Another command is `alias`, which will list all of the current aliases.
