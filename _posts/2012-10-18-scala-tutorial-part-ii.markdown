---
layout: post
title: "Scala Tutorial (Part II)"
date: 2012-10-18 22:14
comments: true
categories: [Scala, Play!]
published: true
---

This is a continuation to [a short Scala tutorial](/2012-10/07/a-short-scala-tutorial-for-java-developers) and [Scala+Play Development Guide](2012/10/15/scala-plus-play-development-guide).

During the development of a project using Typesafe Stack (Scala + Akka + Play!), I learnt quite a few things about Scala and the Play framework.

<!-- more -->

## Switch case statements ##

You can do [switch statements](http://kerflyn.wordpress.com/2011/02/14/playing-with-scalas-pattern-matching/) pattern matching. A simple `switch case` statement looks like this:

	n match {
		case 0 => println("Zero")
		case 1 => println("One")
    	case n => println("It is " + n)
  	}


## What is sealed trait ##

Use [sealed trait](http://stackoverflow.com/questions/11203268/what-is-a-sealed-trait) as `enums`

	sealed trait Answer
	case object Yes extends Answer
	case object No extends Answer



Add a folder to classpath (for reading files). Don't have to put in src.

In Eclipse, right click, Build Path > Use As Source Folder

Folder will be in classpath. Access using





## Read a resource file in Scala Play! 2 ##

Let's say you have a text file call `myfile.txt`. You should put in `/public` of your Play! project.

You can read the file with the following [code](http://stackoverflow.com/questions/12825644/how-to-read-a-file-in-scala-with-play-2-0):

	val is = Application.getClass().getResourceAsStream("/public/myfile.txt")    
	val src = scala.io.Source.Source.fromInputStream(is)
	val iter = src.getLine
	for (s <- iter)
		println(s)




## Accessing Global Object ##

If you have an object that is initiated once when the app starts, you can put it in the `Global` object. 

However, `Global` object is by default in a default package, and because it is in default package, it cannot be referenced/accessed by other packages.

The [workaround](http://stackoverflow.com/questions/10440864/play-2-0-scala-accessing-global-object) is to move your Global object into a specific package, and change the `application.conf` file to

	global= my.packaged.Global



## Initializing a class ##

There is differences between

	class Person(name:String)

and

	class Person(val name:String)

In the later (with `val` in the constructor), you can access `person.name`. For former does not. Very subtle difference until when I create my first class.


## Unit Testing with Specs ##

Write your unit tests with [specs2](http://www.playframework.org/documentation/2.0/ScalaTest).

To run just 1 test:

	sbt test-only test.MySpec



## java.lang.OutOfMemoryError: PermGen space ##

When `sbt run`, sometimes you would run into the error

	java.lang.OutOfMemoryError: PermGen space

To solve, you can

	brew info sbt

to look for a clue. Changing to 512M helps.
	
	export SBT_OPTS="-XX:+CMSClassUnloadingEnabled -XX:MaxPermSize=512M"

As much as you could googled, [some](http://javarevisited.blogspot.sg/2011/09/javalangoutofmemoryerror-permgen-space.html) solutions do not work.



## Split 1 line of code into multiple lines ##

Something so simple does not work as you normally do in Java. For example, this would not compile:

	val s = "a"
		+ "b"

This is because `+` is a method, and it needs to be on the same line as the member calling the method (that is "a").

Instead, this will work:

	val s = "a" +
		"b"

Or you can use brackets..
	
	val s = ("a"
  		+ "b")

The same goes for calling methods with dot notation. This will not work:

	myClass.someMethod
		.anotherMethod

This will work:

	myClass.someMethod.
		anotherMethod



## Play! asText is tricky ##

`request.body.asText` is tricky. It could be None even when there is something.

Let's same you POST some data, and you didn't specify `Content-Type: application/x-www-form-urlencoded`.

Firstly, `request.body.asFormUrlEncoded` will be `None`. Okay, I understand that is because Play! is strict with missing content-type.

However, `request.body.asText` will be `None` too!


## A simple HTTP POST ##

[Dispatch](http://dispatch.databinder.net/) is the most popular HTTP library for Scala. However, to me, it is difficult to understand, [cryptic](http://www.flotsam.nl/dispatch-periodic-table.html), and with poor documentation.

It took me a while to find a [basic use](http://stackoverflow.com/questions/12342062/basic-usage-of-dispatch-0-9), without using those crazy operators:

	val req = url("http://my.server.com/").POST.
	  setBody("yeah").
	  addQueryParameter("foo", "true").
	  addHeader("Content-type", "application/json")

Then gets back a response in blocking way.

	val response = Http(req)()
	val body = response.getResponseBody

HTTP should be that simple, and readable.




