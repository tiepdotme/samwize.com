---
layout: post
title: "Non-ASCII Character in Python File"
date: 2012-09-18 18:00
comments: true
categories: Python
---

If you have non-ASCII characters eg. chinese in your python source code, you would encounter the following error:

	Python Error: Non-ASCII character in file but no encoding declared

Python by default does not allow Non-ASCII characters in the file. You have to insert the following at the top of the Python file.

	#!/usr/bin/env python
	# -*- coding: utf8 -*- 

This works on a [per file basis](http://www.python.org/dev/peps/pep-0263/).