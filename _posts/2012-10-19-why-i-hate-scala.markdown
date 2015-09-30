---
layout: post
title: "Why I hate Scala"
date: 2012-10-19 21:44
comments: true
categories: Scala
published: true
---

I spent about 2 weeks using Scala, before concluding that I hate the language.

When I first wrote the first [beginner guide to Scala](/2012/10/07/a-short-scala-tutorial-for-java-developers/), I was in awe, and yet feeling a tinge of uneasiness. 

Then I spent a few days creating a Scala web application, resulting in a subsequent [random guide to Scala programming](/2012/10/15/scala-plus-play-development-guide/).

After I am done with the project, I know I would not start another project in Scala at my own will.

<!-- more -->

There are many [Scala](http://m-mansur-ashraf.blogspot.sg/2011/08/is-scala-really-too-complex-for-average_20.html) [haters](http://amplicate.com/hate/scala).

Here's what I think:

- Scala is a powerful & complex language. 

- You could have dozens of custom operators like `</>`, and `=:=`. That makes understanding code very difficult. Take a look at the [dispatch](http://www.flotsam.nl/dispatch-periodic-table.html) (HTTP) library.

- Even simple usage requires explaining. There's too many concepts.

- Otherwise, it is a [lack of explaining](http://dispatch.databinder.net/)

- You will be constantly figuring out the language syntax

- Ecosystem not great; lack of support, docs

It's damn hard to read Scala code. 

You can call me an average programmer for all I care. But let's say I am average, and I can't understand Scala, then Scala is not a universal language.

Reading a [real life feedback from Yammer](http://blog.joda.org/2011/11/real-life-scala-feedback-from-yammer.html), it worries me even more. Some other things they said:

- Major version releases is backward incompatible

- High learning curve, slow to get productive

- To increase performance: Don't use for-loop, s.c.m/i.. wtf

- Compared 2 codebases, consensus is Java. Hence they switched back.

I like Python better. And I always like one of [Python design philosophy](http://c2.com/cgi/wiki?PythonPhilosophy)

> There should be one-- and preferably only one --obvious way to do it.

Scala has *too many ways*. 

You will not be able to comprehend easily. Never.