---
layout: post
title: "Common Git Usage"
date: 2012-10-30 21:35
comments: true
categories: [Tools, Git]
published: true
---

I am never good at using Git.

But it's not my fault. Git has too many commands, and I am not a command line guy. That's why I usually use [SmartGit](http://www.syntevo.com/smartgit/index.html), a Git software with user interface that supports Mac.

However, there are times when you have to run on the command line (eg. scripting or you just want to be *real fast*).

These are my common use cases during development work flow. I don't cover [everything](http://ndpsoftware.com/git-cheatsheet.html), or advanced stuff.

<!-- more -->

## Push an existing repos to Github ##

Assuming you have a repos on github already, issue the following [commands](https://gist.github.com/868939):

	cd existing_git_repo
	git remote add origin git@github.com:[user]/[reponame].git
	git push -u origin master



## Setup Github to remember password ##

The [instructions](https://help.github.com/articles/set-up-git). Basically,

	git config --global user.name "Your Name Here"
	git config --global user.email "your_email@youremail.com"

Then download and setup `git-credential-osxkeychain`.

	curl -s -O http://github-media-downloads.s3.amazonaws.com/osx/git-credential-osxkeychain
	chmod u+x git-credential-osxkeychain

Issue a `which git` and note the path of git. Assuming it is `/usr/bin/git`, you have have to move to `/usr/bin/` (IMPORTANT: less the git part!).

	sudo mv git-credential-osxkeychain /usr/bin/




## On Color Options ##

One of the most important [configuration](http://git-scm.com/book/en/Customizing-Git-Git-Configuration)

	git config --global color.ui true

With that, commands like `git diff` and `git log -p` looks better




## What's changed ##

Sometimes, you want to see what are the changes in your working directory (compared to HEAD/the last commit).

	git diff somefile.py

Or you want to see what are the overall changes between the last 2 commits

	git whatchanged -n 1

Then the actual code changes

	 git log -p somefile.py




## Added new files, Updated changes, or Deleted files ##

If you have added new files or updated tracked files

	git add .

If you have deleted files or updated tracked files

	git add -u

You could do both in a single step

	git add -A



## What are my remotes/branches ##

To know,

	cat .git/config

Or you can [list](http://gitref.org/remotes/) the remotes

	git remote -v



## Change remote url

With the list of remotes, you can change to a new URL (http or ssh). Eg. Change `origin` remote to a ssh url:

  git remote set-url origin git@github.com:USERNAME/REPOS.git



## Update your repos ##

It's a 2 step process. First you fetch the changes from a remote named `origin`

	git fetch origin

Then you merge a branch `master` to local

	git merge origin/master

Or Simply

	git pull origin master

If `origin` is a default remote and 'master' is default branch, you can drop it eg. `git pull`.





## Fix merge conflicts ##

This always happen when you work in teams. A very [commmon](http://stackoverflow.com/questions/161813/how-do-i-fix-merge-conflicts-in-git) question.

It usually goes like this.. You tried to update

	git fetch
	git pull
	... not uptodate. Cannot merge.

So you commit your local changes

	git add .
	git commit -m "some changes"
	...
	Automatic merge failed; fix conflicts and then commit the result.

So you need to resolve the conflict

	git mergetool -y

At this point you use the GUI (eg FileMerge) to resolve the conflicts and save. Then you commit

	git add .
	git commit -m "fixed conflicts"
	git push

Done!



## Revert a file during merge conflict ##

Say when you have a merge conflict, you know it should just take your file,

	git checkout --ours filename.c

Or if you know yit should be their file,

	git checkout --theirs filename.c



## Tagging ##

Useful when you want to [tag a version](http://git-scm.com/book/en/Git-Basics-Tagging).

	git tag -a v1.2

If you want to push to the tag, it works similarly like a branch.

	git push origin v1.2



## Delete a commit that has been pushed ##

This is usually when you accidentally commit [wrongly](http://stackoverflow.com/questions/1338728/how-to-delete-a-git-commit).

	git log
	git reset --hard <sha1-commit-id>
	git push origin HEAD --force



## Github.com secret tricks

Read this [cheat sheet](https://github.com/tiimgreen/github-cheat-sheet). You will learn shortcut keys and referencing issues easily.

