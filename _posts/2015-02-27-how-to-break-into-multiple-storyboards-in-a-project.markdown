---
layout: post
title: "How to break into multiple storyboards in a project"
date: 2015-02-27 22:23:13 +0800
comments: true
categories: [iOS]
---

Xcode kind of just want us to have ONE storyboard in a project.

But that is difficult for a team to work on because it is almost impossible to resolve any merge conflicts for storyboard (utterly complex XML).

There are 2 ways.

<!-- more -->

## Pass along a banana

Yeah. Really. Literally.

Have only 1 person to edit the story at any one time.

He who can edit [holds the banana](http://spin.atomicobject.com/2014/02/18/ios-storyboards-xcode5/). Or any other token.


## Multiple Storyboards

An improve to the banana system is to have multiple bananas. Or multiple tokens. So more ~~monkeys~~ developers can work on it at the same time.

This requires you to [decompose your storyboard into modules](http://robsprogramknowledge.blogspot.sg/2012/01/uistoryboard-best-practices.html).

Then make sure of [RBStoryboardLink](https://github.com/rob-brown/RBStoryboardLink) to link the storyboards together.

What is RBStoryboardLink? It links Storyboard A to Storyboard B by placing a *pseudo view controller* in A, with attributes saying what storyboard name and scene name it links to. The magic comes from their custom segues. 

[Unwind segues](http://stackoverflow.com/a/15839298/242682) works too.