---
layout: post
title: "Getting Started with Dotfiles"
date: 2014-01-12 11:44
comments: true
categories: [Mac]
---

It's a new year, and I thought I learn something new to increase productivity.

I stumbled upon **Dotfiles** when watching a [Google I/O 2012 video](https://www.youtube.com/watch?v=Mk-tFn2Ix6g) (awesome video on web tools btw).

Dotfiles are hidden files, such as `.bash_profile` and `.aliases`, which you edited to suit your workflow for your Mac.

Because everyone has their own 'dotfiles', it makes sense to share.

<!-- more -->

Hence some of the [best Dotfiles](http://dotfiles.github.io) are on GitHub.

I recommend starting with [Mathias](https://github.com/mathiasbynens/dotfiles) version, which is a *de facto for Mac OS X*.

# Steps:

1. Backup your current dotfiles. For example, I already have `.bash_profile` and `.gitconfig`, so I copy them to `dotfiles_backup`.

2. Fork [Mathias Dotfiles](https://github.com/mathiasbynens/dotfiles), then `git clone` your fork instead. Highly recommended to fork because you will most likely make some changes.

3. Read through the dotfiles. Example read `.bash_profile`, edit, and also merge from your `dotfiles_backup` as necessary.

4. In the cloned repos, `source bootstrap.sh`.

5. Read through and edit `.osx`, then run `./.osx` once.

Here's [my very own version](https://github.com/samwize/dotfiles) which deactivated some stuff that I don't need, and modified for my needs.
