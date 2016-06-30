---
layout: post
title: "Good Architecture for iOS App"
date: 2016-06-30T17:17:01+08:00
categories: [Architecture]
---

## Good iOS Architecutre

http://slideslive.com/38897361/good-ios-application-architecture-en

by Krzysztof Zabłocki 
You might know him from [KZLinkedConsole](https://github.com/krzysztofzablocki/KZLinkedConsole)
his popular Xcode plugin
[written in Swift](http://merowing.info/2015/12/writing-xcode-plugin-in-swift/)

Good Traits:

- Ability to follow data flow with ease

Limited Dependencies

## How you know it is bad?

    # How many lines in your view controllers?
    find . -type f -exec wc -l {} + | sort -n
    
[Dependency Visualizer](https://github.com/PaulTaykalo/objc-dependency-visualizer)


## Patterns

How you use patterns
determines it is good or bad

Singleton
Very common
Easily misused
Eg. Logger - almost every class needs logging, so singleton is correct

Composition
the 1 pattern to know
which will lead to other great patterns

Eg. Unity

DI goes well
encourage reuse
even Apple encourage in WWDC 2016-06


## MVC

Apple MVC is wrong
View and Controller is too closely coupled

Only model can be reused

From the 80s
But it can still be good
if you do composition

Apple sample code
even they say it is to showz a technology
not good Architecture

## VIPER

Good
But lots of boilerplate
Usually


## MVVM

Most popular iOS Architecture?

View Model
is UIKit independent
is testable

No router


## MVVM+

With
Flow Coordinators

To read: Improving Architecture with Flow Controllers, Coordinator Redux by Khanlou

Does not play well with segue


## ReSwift - new kid in the block

Single point of truth

Dev Tooling?
It can load a state to debug

Code injection n real-time
Must try - save time eg. UI stuff

https://github.com/johnno1962/injectionforxcode
