---
layout: post
title: "Creating a Private Framework With Carthage"
date: 2018-06-27T12:12:15+08:00
categories: [Carthage]
---

## Install

    brew install carthage

## Create a Xcode Project

In Xcode, create a new framework project with **File > New > Project > Cocoa Touch Framework**.

With the project created, click on **Product > Scheme > Manage** and make sure the **Shared** checkbox is enabled.

You will probably want to configure your framework's target under **General**. Change the name, version, and especially the **(minimum) Deployment Target**.

It is a good time to initialize git with `git init` and do your first commit.

## Add your source

This is where you add your source files and assets. Make sure they are added to the framework's target.

If you use any other external frameworks, make sure they are added under the target's **General > Linked Frameworks and Libraries**.

## Add a sample project

You will most likely create a sample project to showcase how the framework is to be used.

Go to **File > New > Target** and choose a suitable one eg. iOS Single View App

Select the target, and under **Build Phases > Target Dependencies**, add your framework.

## Build

Build the framework with carthage,

    carthage build --no-skip-current

The command will build and output the framework in the `Carthage` folder.

Git tag and push now to publish.

    git tag "v1.0.0"
    git push --tags

That't it for publishing your own framework!

This tutorial actually does not restrict to only private. If your git repository is public, then you have a public framework :)

## Using the framework

There are many tutorials on how to use Carthage to install dependency frameworks. I will keep this short.

App that wants to use the framework can add to their [Cartfile](https://github.com/Carthage/Carthage/blob/master/Documentation/Artifacts.md#cartfile) with the repository URL (HTTP or SSH), and version (or tag), like this:

    git "git@bitbucket.org:just2us/MyFramework.git" ~> 0.0.1

Then run `carthage update`.

Unlike Cocoapods, Carthage needs a few more steps to manually [add the frameworks to Xcode](https://github.com/Carthage/Carthage#adding-frameworks-to-an-application).

Drag **Carthage/Build/iOS/MyFramework.framework** into your application target's **General > Linked Frameworks and Libraries**.

Add a **New Run Script Phase** for the application target's Build Phases with

    /usr/local/bin/carthage copy-frameworks

In the same run script, add the **Input Files**,

    $(SRCROOT)/Carthage/Build/iOS/MyFramework.framework

And the **Output Files**,

    $(BUILT_PRODUCTS_DIR)/$(FRAMEWORKS_FOLDER_PATH)/MyFramework.framework

When you add a new dependency, run `carthage update` again, and repeat those manual setup steps.

You might also want to have dependencies for your test targets, in which you can read [here](https://github.com/Carthage/Carthage#adding-frameworks-to-unit-tests-or-a-framework).
