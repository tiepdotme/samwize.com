---
layout: post
title: "How to migrate SVN to Git"
date: 2012-11-28 23:10
comments: true
categories: Git
---

My biggest problem with SVN is that I don't have good Mac software supporting SVN. For Git, I have the free [SourceTree](http://www.sourcetreeapp.com/) app.

Hence I decided to migrate some of my ancient repos that were still on SVN.

It is actually easy.

<!-- more -->

It is basically just 1 command to clone the svn repos to your local working directory. The operation will maintain the logs.

	git svn clone -s http://my.svnserver.com/svn/myapp/ MyAppGit

The `-s` command will clone just the trunk. If you don't use the stand trunk, tag, branch, you can `-s` and it will clone the whole repos.

You can verify all logs are there with `git log`.

Next, maintain the same ignore configurations.

	git svn create-ignore

Lastly, add the new remote to your git repos (eg Github).

	git remote add origin https://github.com/samwize/myapp.git
	git push -u origin master

That's all!

You may also want to refer to [some common Git usage](http://samwize.com/2012/10/30/common-git-usage/).