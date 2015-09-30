---
layout: post
title: "Find all .git in a directory"
date: 2013-05-26 22:02
comments: true
categories: [Git]
---

To find all the git repositories in a directory, you can run the following command (which search for the `.git` directories):

	find . -type d -name .git

<!-- more -->

It is using the `find` command, with the type directory, and name `.git`. 

If you want to delete them, you can do this:

	find . -type d -name .git | xargs rm -rf


PS: The `find` command has a `-delete` option, but that somehow doesn't work with directory.

## Remove all .DS_Store and .gitignore ##

If you are like me who wants to clean up a directory with all the git stuff and send to a possibly Windows user, you could run these:

	find . -name '*.DS_Store' -type f -delete
	find . -name '.gitignore' -type f -delete

The `-delete` option works for the files.