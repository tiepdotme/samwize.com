---
layout: post
title: "Sphinx – Documentation for Python"
date: 2012-09-13 22:08
comments: true
categories: Python
---
Python is a wonderful language also because of the awesome tools that are available.

One of which is [Sphinx](http://sphinx.pocoo.org/). It is like markdown, but even more, with cross referencing of pages and autogeneration of doc for python code.

Start with installing Sphinx.

	$ sudo easy_install -U Sphinx

<!-- more -->

Create sphinx docs in your project.

	$ cd /path/to/project/docs/
	$ sphinx-quickstart

Write your documentation (eg. quick start guide) in your index.rst. Sphinx uses reStructured Text (a kind of text formatting).  Refer to this [quick tutorial](http://thomas-cokelaer.info/tutorials/sphinx/rest_syntax.html) and this [handy cheatsheet](https://github.com/ralsina/rst-cheatsheet).

[Cross reference](http://sphinx.pocoo.org/markup/inline.html#cross-referencing-syntax) your documentation. You can link across to arbitrary location with ``:ref:`my-label-name` `` or across files with ``:doc:`my-doc-name` ``.

	.. _my-label-name:
	 
	Section to cross-reference
	--------------------------
	 
	This is the text of the section.
	 
	It refers to the section itself, see :ref:`my-label-name`.

Finally, generate the HTML files.

	make html

Lastly, Sphinx can automatically generate your module and classes, using your docstrings. That way, you can write your documentation in your python files (and not again in Sphinx doc).

To do that, you have to edit Sphinx's `conf.py` and add your modules path to `sys.path`. Insert the following code in `conf.py`:

``` python
	sys.path.insert(0, os.path.abspath(os.path.join(os.getcwd(), '..')))
```

With that, you can automatically generate doc for a class.

	.. automodule:: mymodule.something
	 
	.. autoclass:: MyClass
	    :members:

This is a short introduction guide to Sphinx. There’s obviously more, with cases of authors writing books using Sphinx!