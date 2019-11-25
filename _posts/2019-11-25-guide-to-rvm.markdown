---
layout: post
title: "A Very Short Guide to rvm"
date: 2019-11-25T22:24:04+08:00
categories: []
---

rvm is a necessary tool to run different version of ruby.

Once in a while I need to upgrade ruby version, but [rvm's official doc](https://rvm.io/rvm/basics) is a bit too dense.

## Install the latest

    rvm install ruby --latest

Or manually specify a version with `rvm install ruby 2.0.0-p247`

## Set the default version

    rvm --default use ruby-2.6.3

The `default` option is often needed, because without it, if you reload the terminal or restart the computer, it will use back the previous.

To confirm the version in use:

    ruby -v
    which ruby

## Other commands

    rvm list
    rvm list known
