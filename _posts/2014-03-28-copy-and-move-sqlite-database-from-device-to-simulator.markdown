---
layout: post
title: "Copy and move sqlite database from device to simulator"
date: 2014-03-28 08:54:08 +0800
comments: true
categories: iOS
---

There are times when you want to have the same database in both a device and a simulator. For my case, I have a photo application which takes photo using the camera. That cannot be nicely tested using a simulator, hence I always test with a real iPhone.

Then comes a time when I want to move the database to simulator, which is faster to run.

This post explain the steps of copying over.

<!-- more -->

## 1. Copy using iExplorer

I use the demo version of [iExplorer](http://www.macroplant.com/iexplorer/) to explore the file system of my iPhone.

Navigate to your app (eg MyApp), and the database is in:

    /Library/Application Support/MyApp/

Copy the 3 database files (MyApp.sqlite, MyApp.sqlite-shm, MyApp.sqlite-wal), and if you use external storage, copy the folder (.MyApp_SUPPORT) too. Note that this folder is prefix with a `.`, hence it will be hidden in your file system.

Make sure these database files are in your computer, as you need to copy them to your simulator.


## 2. Find out the path to your simulator

To know the path to your simulator, you can print a path directory in your app.

For example, the code below I used print the full Document path (using [StandardPaths library](https://github.com/nicklockwood/StandardPaths)):

    NSLog(@"Doc Path: %@", [[NSFileManager defaultManager] publicDataPath]);

This is where my simulator is at:

    /Users/junda/Library/Application Support/iPhone Simulator/7.1/Applications/9B6E8432-1B84-4BB1-B9FF-838DB9DAB915/


## 3. Copy the files over

Copy the database files from (1) to the simulator. Preserve the path, hence the files should be copied to the following:

    /Users/junda/Library/Application Support/iPhone Simulator/7.1/Applications/9B6E8432-1B84-4BB1-B9FF-838DB9DAB915/Library/Application Support/MyApp/

If you use Finder, you can copy the visible files easily. If you have external storage and the folder is hidden, you have to use the command line and `mv .MyApp_Support /to/the/simulator/path/above/`.

That's it!

You can now run on simulator, and it should have the same dataset.
