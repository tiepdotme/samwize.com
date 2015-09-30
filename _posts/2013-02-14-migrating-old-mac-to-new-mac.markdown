---
layout: post
title: "Migrating old Mac to new Mac"
date: 2013-02-14 23:29
comments: true
categories: Mac
---

I did a migration using the **Migration Assistant** app when I bought a new iMac (late 2012 27" model).

I decided to migrate everything (instead of a fresh install) as I DON'T want to:

- reinstall hundreds of apps
- setup the system preferences 
- manually transfer files

But migration is not perfect (though I think it is much efficient than doing a fresh install). 

Here are some tips that you will be interested to know.

<!-- more -->

## 1. Create a new user with a different username ##

In your new iMac, when you start the OS the first time, it is compulsory for you to create a new user account. 

This is temporary. Once you migrated, the account will be deleted. 

Don't create a user account with the same username as that from the old Mac, as that will cause a conflict, and you have to start over at some point in the migration. This tip could save you 10 minutes.


## 2. Transfer medium using cross cable ##

This is up to you. For me, I choose to connect the 2 Macs using a cross ethernet cable.

It took 8 hours to migrate nearly 500 GB of data.


## 3. Most things are good ##

Once migrated, most things are good.

Your applications are there. Your OS settings are intact. Your files are there.

In the next sections, it tells some of the things that were broken.


## 4. Dropbox ##

Your dropbox folder is there, but it is not synced. 

You have to reinstall Dropbox, and they will resync like old times.


## 5. Git ##

Your `git` command cannot be recognized. Simply [download](http://git-scm.com/download/mac) and install again.


## 6. Octopress ##

It is best you go through this [octopress setup guide](http://samwize.com/2012/09/11/how-to-setup-octopress-on-github-pages/) again. 

Explicitly, you do need to install OS X GCC Compiler and bundle install.


## 7. Eclipse, and Plugins ##

If you open and encounter an error, simply [reinstall](http://www.eclipse.org/downloads/). You need to install Google Plugin for Eclipse too if you are using.


## 8. More.. ##

More to be updated as I use my new iMac :)
