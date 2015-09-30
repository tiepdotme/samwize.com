---
layout: post
title: "Use Async Library to Improve your spaghetti-nested-asynchronous-functions"
date: 2014-01-27 22:14:56 +0800
comments: true
categories: Node
---

One of the most terrible thing about Node.js is that almost every function is asynchronous.

Unlike synchronous programming where you have results returned right from a function call, in asynchronous programming, you need to deal with a lot of callbacks.

Fortunately, [async library](https://github.com/caolan/async) comes to the rescue.

<!-- more -->

Let's look at an example of a highly nested operation where you need to read some file, make a directory, then write some file.

```javascript
fs.readFile('somefile.txt', function(err, data) {
	// Use data and do something..
	fs.mkDir('somedirectory', function(err) {
		fs.writeFile('anotherfile.txt', 'some stuff to write', function(err) {
			// Done!
		});
	});
});
```

Okay, just 3 nests. But I hope you understand how terrible asynchronous programming is.

With async library, you can improve your code. The example above actually requires a [waterfall](https://github.com/caolan/async#waterfall) flow, in which each function pass some data to the next function in a sequential way.

This is how you write with async:

```javascript
async.waterfall([

	// Task 1
	function(callback) {
		fs.readFile('somefile.txt', function(err, data) {
			callback(null, data);
		}
	},

	// Task 2
	function(data, callback) {
		// Use data and do something..
		fs.mkDir('somedirectory', function(err) {
			callback(null);
		}
	},

	// Task 3
	function(callback) {
		fs.writeFile('anotherfile.txt', 'some stuff to write', function(err) {
			callback(null);
		}
	}

], function (err) {
	// Done!
	// Or if any error encountered along the way!
});
```

While at first glance, you will think why write more code with async!?

While there are more code, but it is _neater code_.

Imagine if you have 10 tasks instead of 3 tasks. Using async will be much much neater. What's more, there are more [control flows](https://github.com/caolan/async) that async provides.

If you program node.js, I suggest you get started with async library as early as possible.
