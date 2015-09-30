---
layout: post
title: "Python Unit Testing with nosetests"
date: 2012-11-12 22:52
comments: true
categories: Python
---

[nosetests](http://nose.readthedocs.org/en/latest) is Python favorite unit testing framework.

It helps to collect tests from functions that has the word `test`, or subclasses from `unittest.TestCase`, etc.. Learn the [different ways](https://nose.readthedocs.org/en/latest/writing_tests.html) that you can write test cases.
   
<!-- more -->

## How to use ##

Install,

	sudo pip install nose

Then run,

    nosetests
    
nosetests will automatically find tests to run in your project! Not necessary in /tests directory (but recommended).

To print out the output,

	nosetests --nocapture

To run only certain tests,

	nosetests <path/to/file>:<Test_Case>.<test_method>
    nosetests test_web.py:TestWeb.test_checkout

To run a test file,

	nosetests test_web.py


