---
layout: post
title: "Backing Up Photos Library and Handling Corrupted Files Error -36"
date: 2017-06-26T00:03:39+08:00
categories: []
---

I was backing up my Photos Library, a huge 500 GB `Photos Library.photoslibrary`, and copying the file to my external drive, it had this error:

> The Finder can’t complete the operation because some data in Photos Library.photoslibrary” can’t be read or written. (Error code -36)

As ambiguous as it can be, error code 36 turns out to be I/O problem, or corrupted files.

For my case, it is most likely a corrupted photo/video. I have no idea how they became corrupted in Photos. Either Photos app is buggy and corrupted it, or my harddrive is faulty..

In any case, I have to recover them.

[MacWorld has a tip](http://www.macworld.com/article/3189985/photography/how-to-fix-an-uncopyable-iphoto-or-photos-library.html) on creating a new "photoslibrary", revealing the package, and manually copying the original files to the new directory. Doing so will reveal the exact files that are throwing the error.

But if you have lots of photos/videos and there are lots of corrupted files, then you will save lots of time if you know how to use the command line.

```bash
ditto -v ~/Pictures/Photos\ Library.photoslibrary/ /Volumes/MyDrive/Photos\ Library.photoslibrary/
```

The `ditto` command will copy without stopping on failure. Instead, it will log the files with problem.

## Copying Corrupted Files

The above will skip the corrupted files. Not a big deal if you can afford losing some of the photos.

But some of these corrupted files are just partially corrupted eg. a video can be played ok in the first 50%

There is still a way to copy the corrupted files.

[`dd`](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/dd.1.html) let you copy bit by bit, even if an input error occurs:

```bash
dd if=myfile.MOV of=/Volumes/MyDrive/myfile.MOV conv=noerror,sync
```

The above will copy "myfile.MOV" even if it is corrupted.

## Check your disk health status

Using [Disk Utility](https://support.apple.com/kb/PH22243?locale=en_SG), run "First Aid" to repair. Sometimes you may need to boot into [Recovery mode](/2017/04/06/guide-to-boot-up-macos-in-other-modes-and-troubleshooting/) to run.

Or run `/sbin/fsck -fy` (might need to be in [Single User mode](/2017/04/06/guide-to-boot-up-macos-in-other-modes-and-troubleshooting/)), which does the same as First Aid.

## Backing up frequently is important

It is my fault for losing my dear photos, though I also blame my 5 year old iMac (2012 model) which seems to be failing. It is either the OS fault or the drive is having bad sectors.

But it is user's fault for not having a backup plan. I backup but only occassionally, and manually by copying to another external drive.

I didn't use Time Machine, because I didn't have a hard disk that is big enough for my near 1 TB of data on iMac.

I have since bought a [3 TB external drive](http://amzn.to/2tblyR0) at $102 USD. This should serve well as a Time Machine backup drive for the next few years.
