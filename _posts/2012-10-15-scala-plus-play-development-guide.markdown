---
layout: post
title: "Scala + Play! Development Guide"
date: 2012-10-15 16:02
comments: true
categories: [Scala, Play!]
published: true
---

This is a guide on using Typesafe Stack (basically on Scala + Play! Framework).

It covers installation, start a project, setting up Eclipse and Git, then deploying to Heroku.

<!-- more -->

## Installation ##

Install the [stack](http://typesafe.com/stack/download). 

	$ brew install scala sbt maven giter8

`sbt` is the [Simple Build Tool](http://typesafe.com/technology/sbt) for managing Scala project. The sbt-version for this guide is 0.12.0.

`giter8` is for [generating template projects](https://github.com/n8han/giter8).

If you have a 404 error when maven is being installed, you can `brew edit maven` and [change the URL](http://stackoverflow.com/questions/12757694/brew-install-maven-404-error).

## Create a new project ##

Create a new project using `giter8` templates

	$ g8 typesafehub/play-scala


## Run the project ##

Issue the 2 commands to run

	$ sbt
	$ run

The web app will serve at http://localhost:9000/. 

You could as well issue a single command `sbt run`.


## Run a console ##

You can also run a play console to do some testing.

	$ sbt console

In the console, you could call any piece of your code directly and conveniently test out stuff.


## Setup Eclipse ##

In order to open the project using Eclipse, do a 

	$ sbt eclipsify

You would of course download the [Scala IDE](http://typesafe.com/stack/scala_ide_download) (Eclipse). I would rename to `Eclipse-scala` and put in my Applications folder.

Open Eclipse, go to File > Import > General/Existing Project and select the scala project. 

Important: Everytime you added libraries and dependencies to the project, you need to `sbt eclipsify` again.


## Git setup, and .gitignore ##

These are the files to ignore; the content of `.gitignore`:

	logs
	project/project
	project/target
	target
	tmp
	.history
	/.settings/
	/.target/
	/bin/
	/eclipse/
	/.project
	/.classpath
	/.cache
	/.DS_Store

Then do `git init` and your first commit!

	git init
	git add .
	git commit -m "Initial Commit"


## Scala ##

Before the next section on Play! Framework, make sure you are comfortable with Scala first. 

Even how much you about Scala being an easy to understand language, and much like Java, trust me. You need to read an introduction guide to Scala first.

A good starting point would be a [Scala tutorial](/2012/10/07/2012-10-07-a-short-scala-tutorial-for-java-developers/) from me (:


## Play! Framework ##

It's time to start actual development of your app.

However, I am going to cut short here, as this post is merely a short guide to get you started.

Head over to [Play! framework documentation](http://scala.playframework.org/documentation/) for a tutorial. (You may skip the setup portion, and start with the [Action](http://scala.playframework.org/documentation/2.0.4/ScalaActions))


## Deploy to Heroku ##

Heroku supports Play! 2 and Scala. A [wiki](https://github.com/playframework/Play20/wiki/ProductionHeroku) on how to deploy to Heroku is available.

You would firstly need to [register](http://heroku.com/signup) a Heroku account, and then install [Toolbelt](http://toolbelt.heroku.com/).

You must have setup git as described in the previous section.

Create a heroku app and push to the server

	heroku create
	git push heroku master

It will take quite some time to setup the heroku server on the first push. So wait till it says finish, and off you go!


