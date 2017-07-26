---
layout: post
title: "How to Use Git Bisect to Find the First Bad Commit"
date: 2017-07-27T16:47:55+08:00
categories: [Git]
---

Git has a very powerful command for developers to **systematically find the commit** that introduce a bug.

You mark commits as `good` OR `bad`, and using divide-and-conquer strategy, you will find the first bad commit in a few pass (even with hundreds of commits).

## Steps

    # 1. Mark current branch that has the bug
    git bisect bad

    # 2. Mark a tag that is working, or use a commit SHA directly
    git bisect good v1.2.3

    # 3. Git will automatically checkout the commit between the good and bad
    #    You will have to identify if that commit is good or bad, and mark with 
    git bisect bad
    # OR
    git bisect good

Eventually, you will find the first bad commit.

You can return to your previous branch with `git bisect reset`.

For more features, refer to [git-scm doc](https://git-scm.com/docs/git-bisect/).

## What if you are looking for the first change?

Sometimes, you are not finding the first commit that is **bad**.

Rather, you want to find the first commit that has **new changes**.

Since Git v2.7.0, they introduced `new` and `bad` as new terms. If you want to use your own terms, or if you are using older version of git, you can define your terms.

Example to have  `fixed` and `unfixed`,

    git bisect start --term-new=fixed --term-old=unfixed

Then mark with,

    git bisect fixed
    git bisect unfixed

## What is the very first commit SHA?

The "Initial Commit" SHA can be retrieved with:

`git rev-list --max-parents=0 HEAD`
