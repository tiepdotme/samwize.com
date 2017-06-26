---
layout: post
title: "Guide to Booting Up macOS in Other Modes, and Troubleshooting"
date: 2017-04-06T23:02:29+08:00
image: /images/macos-bootup-hang.jpg
categories: [mac]
---

This guide is for troubleshooting macOS in the scenario that it can't boot up.

I have personally encountered a couple of times, frightening scenarios, where my mac somehow could not boot up, or get stuck during login etc. macOS provides many "secret" modes to help to troubleshoot.

Knowing them will be handy in time of crisis.

You will need to hold down certain keys when your machine is powered on to enter these modes. When you hear the startup sound and see the Apple logo, you may release the keys.

## Resetting PRAM/NVRAM

_Hold Command + Option + P + R_

Parameter RAM stores default values, and it could get corrupted for some reasons.

This is **The Number 1** troubleshooting resolution. Always try this first.

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

In single user mode, you have access to terminal, so you can run certain tools to troubleshoot.

```bash
# Check and repair file system for consistency
/sbin/fsck -fy
```

## Apple Hardware Test

_Hold D_

You can perform a short test (around 3 min), or an extended test (around 1 hour).

Special case: If you encountered `4HDD/11/40000000: SATA(0,0)`, it could be a [false positive](https://support.apple.com/en-sg/HT203648). You can disable looping by pressing _L_ before starting the test. I encountered this, but I still managed to "fix" it - the SSD is not damaged in my case. So don't worry too much on this error.

## Recovery Mode

_Hold Command + R_

If all else failed, you can try reinstalling macOS.

## Startup Option

_Hold Option_

You can startup from other device such as external drive. It is possible to load the entire OS in an external drive, and boot from there.

--- 

## Common Bootup Errors & Fixes

`Mac OS version not yet set` - Try boot with startup option (_Hold Option_) and select the disk with the OS

If you have tried all ways and your Mac OS still refuses to boot, you can create a [bootable macOS on USB](https://support.apple.com/en-sg/HT201372). Then _hold option_ while bootup to select the OS on USB.
