---
layout: post
title: "Squashing Multiple Commits into One"
date: 2015-08-22 13:30:02 +0800
comments: true
categories: [Git]
---

A good tip for developers using Git is to squash multiple commmits into one.

Why do that?

If you ever work on a task and had multiple commits because you want to be _safe_ and track the changes while work-in-progress, your commit history will be difficult to follow.

It will be hard (and waste of time) for code reviewer, or other developers, to go through multiple commits. There will be bound to be codes that you add, then remove or edit.

<!-- more -->

To [squash the last 5 commits](http://ponyfoo.com/articles/git-github-hacks), this is what you do:

    git reset HEAD~5
    git add .
    git commit -am "Here's the bug fix that closes #28"
    git push --force

If you are using SourceTree, you can right click on the last commit before yours, select "Reset master to this commit", using the mode "Mixed". Then re-commit those changes again.

