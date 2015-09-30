---
layout: post
title: "Octopress would not deploy"
date: 2012-09-30 21:59
comments: true
categories: Octopress
---

I encountered a problem when I did a `rake deploy`. Somehow it does not get deployed. 

The only error I got was:

	The following paths are ignored by one of your .gitignore files:
	_deploy

<!-- more -->

That's a strange error. And I verified the `public` and `_deploy` directories were generated correctly.

It turns out to be an issue when you clone the repos. You [must setup the Github pages](https://github.com/imathis/octopress/issues/412
) again.

	rake setup_github_pages 

Just that. Wasted a few hours. Hopes this help others.


## Another Error: _deploy not found

If you deploy and there is error complaining:

    Errno::ENOENT: No such file or directory - _deploy

You will have to git clone the master branch into _deploy first.

    git clone https://github.com/samwize/samwize.github.com.git _deploy

