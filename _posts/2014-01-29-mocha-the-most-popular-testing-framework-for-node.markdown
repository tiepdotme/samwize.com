---
layout: post
title: "Mocha - the most popular testing framework for node"
date: 2014-01-29 23:38:03 +0800
comments: true
categories: Node
---

[Mocha](https://mochajs.org) is a simple yet wonderfully designed testing framework for Node.js.

In agile development, developers write tests _before_ implementing a feature. When you are [discussing](http://redotheweb.com/2013/01/15/functional-testing-for-nodejs-using-mocha-and-zombie-js.html) a story for a feature, you should already be writing the test cases.

<!-- more -->

Here is a [guide by Brian Stoner](https://brianstoner.com/blog/testing-in-nodejs-with-mocha/). After installing mocha and such, you can create a `MakeFile` like this:

```
REPORTER = dot

test:
  @NODE_ENV=test ./node_modules/.bin/mocha \
    --reporter $(REPORTER) \

test-w:
  @NODE_ENV=test ./node_modules/.bin/mocha \
    --reporter $(REPORTER) \
    --growl \
    --watch

.PHONY: test test-w
```

(In case you [wonder what .PHONY is](http://stackoverflow.com/questions/2145590/what-is-the-purpose-of-phony-in-a-makefile)..)

You can run your tests with:

```
$ make test
```

Another javascript library you should use together with mocha is [should.js](https://github.com/shouldjs/should.js).

Should.js replaces `assert` library with more human-like language eg. `foo.should.be.exactly(5)`

An interesting thing to note: should.js extend object prototype!! While this is usually dangerous, should.js extend object with a _should_, and only that, thus safer.
