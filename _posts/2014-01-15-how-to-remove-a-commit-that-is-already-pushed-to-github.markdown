---
layout: post
title: "How to remove a commit that is already pushed to Github"
date: 2014-01-15 22:02:24 +0800
comments: true
categories: [Octopress, Git]
---

I ran into a situation when I wrongly pushed some commits onto a repository.

Hence I need to remove those commits.

<!-- more -->

The exact situation is all because I didn't fully understand Octopress.. I was [using Octopress on GitHub Pages](http://samwize.com/2012/09/11/how-to-setup-octopress-on-github-pages/).

Usually, you will only be on `source` branch. When you want to push to the `master` branch, you use the `rake gen_deploy` command, which will generate the static files to `_deploy` directory, which will then automatically push `_deploy` onto GitHub.

I have itchy hands..

And switched to the `master` branch..

And there I somehow made 2 commits, and pushed them.

After going back to `source` branch, subsequent `rake gen_deploy` will give this error: 

	failed to push some refs to 'git@github.com:samwize/samwize.github.com'

I don't really understand why. 

But I knew I shouldn't push the 2 commits in the first place.

### Steps to remove the 2 commits

Firstly, find out the comit that you want to revert back to. 

	git log

For example, commit 7f6d03 was before the 2 wrongful commits.

Force push that commit as the new master:

	git push origin +7f6d03:master

The `+` is interpreted as forced push.


### Another way 

You can also use [git reset](http://git-scm.com/book/en/Git-Basics-Undoing-Things) to undo things. Then force push.

	git reset 7f6d03 --hard
	git push origin -f