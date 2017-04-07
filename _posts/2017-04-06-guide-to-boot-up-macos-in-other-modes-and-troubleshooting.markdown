---
layout: post
title: "Guide to Boot Up macOS in Other Modes, and Troubleshooting"
date: 2017-04-06T23:02:29+08:00
categories: []
---

This guide is for troubleshooting macOS in the scenario that it can't boot up. 

macOS provides many "secret" modes. You will need to hold down the keys when your machine is powered on. When you hear the startup sound and see the Apple logo, you may release the keys.

## Resetting PRAM/NVRAM

_Hold Command + Option + P + R_

Parameter RAM stores default values, and it could get corrupted for some strange reasons. The number 1 troubleshooting resolution.

## Resetting SCM

Similar to resetting PRAM, but SCM involves cutting off power, hence it is different for MacBook and iMac. Read [this](https://support.apple.com/en-sg/HT201295).

## Verbose Mode

_Hold Command + V_

If resetting fails, use verbose mode to identify what is causing the problem.

## Safe Mode

_Hold Shift_

This will boot without loading third-party drivers and startup programs.

## Single User Mode

_Hold Command + S_

In single user mode, you have access to terminal, and you can run certain tools to troubleshoot.

```bash
# Check and repair file system for consistency
/sbin/fsck -fy
```

## Apple Hardware Test

_Hold D_

You can perform a short test (around 3 min), or an extended test (around 1 hour).

If you encountered "4HDD/11/40000000: SATA(0,0)", and could be a [false positive](https://support.apple.com/en-sg/HT203648). You can disabled looping by pressing _L_ before starting the test.

## Recovery Mode

_Hold Command + R_

If all else failed, you can try reinstalling macOS.

## Startup Option

_Hold Option_

You can startup from other device such as external drive. It is possible to load the entire OS in an external drive, and boot from there.
