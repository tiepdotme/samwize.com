---
layout: post
title: "What mongoose never explain to you (on case sentivity)"
date: 2014-03-07 15:59:24 +0800
comments: true
categories: Node
---

[Mongoose](http://mongoosejs.com) is the most popular library for using Mongodb on Node.js.

I took some time to learn about the case sentivity and model name renaming it does behind the curtain.

<!-- more -->

Let's assume the model I have is 'Campaign'.

- mongodb collection name is case sensitive ('**C**ampaigns' is different from '**c**ampaigns')

- mongodb best practises is to have all lower case for collection name ('campaign**s**' is preferred)

- mongoose model name should be singular and upper case ('**C**ampaign')

- mongoose will lowercase and pluralize with an 's' so that it can access the collection ('**Campaign**' >> '**campaigns**')

Knowing this is especially useful if you are dealing with existing collections.
