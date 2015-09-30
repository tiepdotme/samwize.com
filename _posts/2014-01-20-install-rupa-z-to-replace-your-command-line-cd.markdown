---
layout: post
title: "Install Rupa z to replace your command line CD"
date: 2014-01-20 18:34:45 +0800
comments: true
categories: Mac
---

[z](https://github.com/rupa/z) is a command line script to help you "jump around" to directories.

Very much like `cd` - change directory - command, but much more powerful. Watch [z in action](http://www.youtube.com/watch?v=f7AU2Ozu8eo&feature=youtu.be&t=2m23s).

<!-- more -->

### Installation

Since I installed [oh-my-zsh](/2014/01/15/getting-started-with-zsh/), I can install it easily be editing the `~/.zshrc`. Simply add `z` as a plugin. Here's mine plugins:

	plugins=(git osx python sublime z)

### How to use

It takes some time for z to learn your directories. So you will need regular `cd` before you can `z` around.

Suppose you have a project directory at `~/Workspace/nodejs/heroes/ironman/`.

	# cd to the path first
	cd ~/Workspace/nodejs/heroes/ironman/

Now, z understand you have such a path, and is able to let you jump around easily from *anywhere*.

	z iron

The above will jump to `ironman` directoy right away.

In fact you `z node`, `z heroes`, or `z man`  will jump to the same `ironman` directory.
