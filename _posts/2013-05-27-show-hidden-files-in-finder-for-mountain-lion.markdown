---
layout: post
title: "Show hidden files in Finder for Mountain Lion"
date: 2013-05-27 22:20
comments: true
categories: [Mac]
---

You can show hidden files in Finder on your Mountain Lion (or any Mac OS).

On Terminal, run this:
	
	defaults write com.apple.Finder AppleShowAllFiles YES

<!-- more -->

Right click on Finder, and select Relaunch.

Voila!

Simlarly, to hide the files in Finder, run this:

	defaults write com.apple.Finder AppleShowAllFiles NO

## Bonus: Find all the big files ##

One of the reason to show the hidden files is to find all the big files that occupy your disk space. 

An easy way is use Finder Search.

- Hit Command+F in Finder to bring up the search. 

- Click on Others > File Size

- Enter the search criteria eg. greater than 1MB

{%img center http://cdn.osxdaily.com/wp-content/uploads/2012/04/find-large-files-mac.jpg %}