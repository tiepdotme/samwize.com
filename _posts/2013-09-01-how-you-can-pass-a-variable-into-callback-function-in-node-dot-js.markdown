---
layout: post
title: "How to pass a variable into a callback function in Node.js"
date: 2013-09-01 18:42
comments: true
categories: [Node]
---

Because Node.js works **asynchronously**, things might not work as you think, if you are still stuck in the **synchronous** world..

To pass variables into callback function, you can use `bind()`. 

I will show you how.

<!-- more -->



# The dummy function that takes 1 sec to callback #

To begin, let's say we have this `dummy()` function that will do some lengthy processing, and then callback. This is common. For example, a [simple http request](/2013/08/31/simple-http-get-slash-post-request-in-node-dot-js/) will use a callback function for the http response. 

```js
// A dummy function that callback after 1 second (simulating some processing work)
function dummy(i, callback) {
	setTimeout(function() {
		// After 1 second, we callback with a result
    	callback('dumb result')
    }, 1000);
}
```


# Incorrect Code #

Now, let's have a code that loops 10 times. 

```js
// Incorrect version
for (var i = 0; i < 10; i++) {
	dummy(i, function(response) {
		console.log("i = " + i + " , response = " + response);
	})
}
```

You might think the code above will print out `i` as 1-10. 

But wrongly, it will print 10 every time!

This is because the callback from `dummy()` takes 1 second before calling back. So by the time we print out, `i` is already 10.




# Correct Code #

There is the **correct way** to pass variables such as `i` into the callback function.

```js
// Correct version
for (var i = 0; i < 10; i++) {
	dummy(i, function(response) {
		console.log("i = " + this.i + " , response = " + response);
	}.bind( {i: i} ))
}
```

As you can see, I have used `bind()` to pass the variables to `function(response)`.

Then in the callback, I can access the variable with `this.i`.


