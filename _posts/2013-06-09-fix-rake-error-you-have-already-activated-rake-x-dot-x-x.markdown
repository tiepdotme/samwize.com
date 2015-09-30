---
layout: post
title: "Fix rake error: You have already activated rake x.x.x"
date: 2013-06-09 22:55
comments: true
categories: [Octopress]
---

I encountered the following error when I try to create a new post in Octopress:

	You have already activated rake 10.0.4, but your Gemfile requires rake 0.9.2.2. Using bundle exec may solve this.

<!-- more -->

This happened because I updated rake.

To solve, simple open `Gemfile`, and update the version number to the latest activated version on your system. eg. 

 	gem 'rake', '~> 10.0.4'