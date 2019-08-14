---
layout: post
title: "Catalina Beta Bug: Deleting Files Did Not Free Up Disk Space!"
date: 2019-07-03T20:19:00+08:00
categories: [macOS, bug]
---

I was on macOS 10.15 Catalina BETA 1, and I encountered a very troubling bug.

I know the risk of beta.. this post is to help others who are in the same situation.

## The Problem: Disk Space Never Free Up

It turns out there is [a bug](https://forums.developer.apple.com/thread/117223) with Catalina together with Time Machine.

Deletion (and emptying thrash) will not return you the disk space.

I was very anxious because I was going to update to a new beta 3, and I thought the new beta should fix the problem. But, I don't have enough disk space to install the new beta! It is a chicken and egg story?

## The Solution

Turns out, it is possible to free up the disk space despite the bug on Beta 1.

You have to **delete Time Machine local snapshot**. I will explain what they are in later section.

Go to command line and list them.

```bash
tmutil listlocalsnapshots /Volumes/
```

My output looks like this:

```
com.apple.TimeMachine.2019-07-02-204331
com.apple.TimeMachine.2019-07-03-085015
com.apple.TimeMachine.2019-07-03-103553
```

Go on and delete a snapshot with the date.

```bash
tmutil deletelocalsnapshots 2019-07-02-204331
```

This will free up a lot of disk space! Do it for all if needed!

If you have many snapshots from the list, you can delete all with `for d in $(tmutil listlocalsnapshotdates); do sudo tmutil deletelocalsnapshots $d; done`.

## What is Local Snapshot?

There is one thing I learnt from this episode.

[Local snapshots](https://support.apple.com/en-us/HT204015) are created on your Mac (NOT on your backup disk), when the backup disk is unavailable.

In some sense, you could even recover from these local snapshots when your backup disk is not around.

However, they are kept at most 24 hours.

This is a good trick when you need to free up some critical disk space.

To disable local snapshot, you can `sudo tmutil disablelocal`.
