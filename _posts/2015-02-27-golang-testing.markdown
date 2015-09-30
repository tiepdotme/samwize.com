---
layout: post
title: "Golang Testing"
date: 2015-02-27 02:01:37 +0800
comments: true
categories: [Golang]
---

Go has a lightweight testing framework.

Their doc covered [how you can start writing your tests](https://golang.org/doc/code.html#Testing). And you can read more on the [testing package](http://golang.org/pkg/testing/).

But there are a few important things they didn't tell you.

<!-- more -->

## Run All Tests

To run tests (in the directory you are in), simply run `go test`.

To specify a package directory, you have to run `go test github.com/user/stringutil`. But that does not include tests in subdirectories.

To run all tests in all package directories including the subdirectories, you have to run:

    go test ./...


## Tests are run in package directory

But note: all tests are run with the [package directory as the working directory](http://stackoverflow.com/questions/23847003/golang-tests-and-working-directory).

In other words, take note if you are using `os.Getwd()` or equivalent.

You mind think the working directory is where you run `go test ./...`. But no. The working directory is the directory that the test is in.
