---
layout: post
title: "Guide to Hosting Your Own Private Cocoapods Framework"
date: 2015-10-09T01:06:49+08:00
categories: [iOS]
---

This is a guide to creating your own Private Cocoapods -- that is hosting your private "Specs" with your private library. 

This is useful for projects that are not open-sourced.



## 1. Setup Your Library

Create your private library with [pod lib create](https://guides.cocoapods.org/making/using-pod-lib-create.html).

    pod lib create MyLibrary

The interactive prompt will help in setting up your library.

The following 2 folders are important:

- Pod - This is where you place your library's classes and assets
- Example - This is the generated Demo & Testing bundle

When you open Xcode, you will find your library source under **Pods > Development Pods > MyLibrary**.

> Development Pods are different from normal CocoaPods in that they are symlinked files, so making edits to them will change the original files, so you can work on your library from inside Xcode. When you add new/existing files to Pod/Classes or Pod/Assets or update your podspec, you should run `pod install` or `pod update`.



## 2. Git Push Your Library

Create your private repository for this library.

Then edit your Podspec file with the repos URL:

    s.source           = { :git => "git@bitbucket.org:myorg/mylibrary.git", :tag => s.version.to_s }

You will need to commit, and tag the version (default starts with "0.1.0").

Finally, `git push`.




## 3. Setup Your Specs Repo

Now, you have to [create your private spec repo](https://guides.cocoapods.org/making/private-cocoapods). This is like a [private trunk](https://guides.cocoapods.org/making/getting-setup-with-trunk.html).

Create another private repository (this is different from step 2). 

Then add this private repos to your system:

    pod repo add MySpecs git@bitbucket.org:myorg/myspecs



## 4. Add your Podspec to Your Private Specs Repo

Go back to your private library.

Push the podspec to your spec repo.

    # Verify correctness of the podspec
    pod lib lint --verbose
    # Push the podspec
    pod repo push MySpecs MySwift.podspec --verbose



## 5. Using the Library

The `Podfile` must now contain the [source](https://guides.cocoapods.org/syntax/podfile.html#source). We now have 2:

    source 'git@bitbucket.org:myorg/myspecs'
    source 'https://github.com/CocoaPods/Specs.git'

That's it. Add the library normally.

    pod 'MyLibrary'

