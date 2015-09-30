---
layout: post
title: "Getting started with node.js scripting"
date: 2013-08-29 23:07
comments: true
categories: [Node]
---

This is a very small getting started guide for myself, when once-in-a-while I will write some node scripts.

<!-- more -->

## Generate package.json ##

	npm init


## Install Dependencies Libraries ##

Node.js is famous for the libraries available. A few common ones are [`request`](https://github.com/mikeal/request) and [`eyes`](https://github.com/cloudhead/eyes.js). 

	npm install request --save
	npm install eyes --save

I usually install them with `--save` argument so that the dependency is written to `package.json`.

When you install a library, it will be added to `node_modules` folder. This folder can safely be gitignored.

On a clean clone of the repos, `package.json` can help to install all libraries at once. Just run:

	npm install


That's all for now.
