---
layout: post
title: "Difference between Git checkout, revert and reset"
date: 2014-01-25 01:11:35 +0800
comments: true
categories: Git
---

These 3 commands that can change/rollback/revert/update a file in your git repository. Whatever you call it.

This post note their differences and their usage.

<!-- more -->

Firstly, the usage is usually

	git checkout/revert/reset myfile

## Checkout

- Use this to checkout a branch or specific commit
- This will rollback any changes you have in your working copy
- This will NOT make any changes to the history

## Revert

- Use this to rollback changes you have committed
- This adds a NEW commit, hence changes the history

## Reset

- Use this to rollback changes made to the index (eg from `git add`), which will NOT change the history
- Use this to change what commit a head is pointing to, which will change the history

Or you may read [this](http://stackoverflow.com/a/8358039) and [this](http://stackoverflow.com/a/1146981/242682) on StackOverflow.