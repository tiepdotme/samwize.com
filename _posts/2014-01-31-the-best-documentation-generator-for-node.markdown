---
layout: post
title: "The best documentation generator for Node"
date: 2014-01-31 16:21:45 +0800
comments: true
categories: Node
---

After researching on the various modules available, I find the best documentation generator is [JSDoc](http://usejsdoc.org), and also [Docco](http://jashkenas.github.io/docco/).

Yea, you can use both simultaneously.

<!-- more -->

## Why Both ?

JSDoc is regular API documentation that explain the methods and modules of your project. You annotate your source code with `@param` etc to explain them. It is good for public API documentation.

Docco is a 2 vertical layout documentation with **prose** and the **code**. It has recently became popular and a good example is [underscore documentation](http://underscorejs.org/docs/underscore.html). It is more suitable for explaining the flow of your code. It is good for small team, or ownself.

Docco is actually very simple. It parses your source code, and any comments that starts with `//` is the **prose**. You can use markdown too. And, it doesn't consider `/** comments */` as a prose, which is good, because JSDoc uses them.

I like to use both simultaneously because

- Docco is great for reading your source code with the comments right beside. It helps in explaining the flow.

- JSDoc is great for looking up on how to use a method, the parameters and type.

[Setting up Docco](http://jashkenas.github.io/docco/) is easy so I will not be explaining.

But ironically, the documentation from JSDoc is not the clearest of all.

## Setting up JSDoc

There are 2 places for their documentation - [usejsdoc.org](http://usejsdoc.org) and [github](https://github.com/jsdoc3/jsdoc).

Firstly, go to [github](https://github.com/jsdoc3/jsdoc) to understand how to install the module.

I made a mistake to install the module globally with npm `-g`, which is WRONG. Install locally with:

```
npm install jsdoc
```

This is because you need to edit the config file in the module at `./node_modules/jsdoc/config.json`. And this part of the information is at [usejsdoc.org](http://usejsdoc.org/about-configuring-jsdoc.html)..

To generate, run this:

```
./node_modules/.bin/jsdoc yourJavaScriptFile.js
```

The doc will appear in `./out` (default directory).


## Using Docstrap

The default theme of JSDoc isn't attractive. Fortunately there is [docstrap](https://github.com/terryweiss/docstrap).

I like [amelia](http://terryweiss.github.io/docstrap/themes/amelia/) (retro feel) and [cerulean](http://terryweiss.github.io/docstrap/themes/cerulean/) (twitter like).

Again, the instructions wasn't very clear.

To use docstrap, you have to `npm install ink-docstrap`, then edit `config.json` in *jsdoc*, and add the following to `templates` key.  Note the `theme` used is cerulean below.

```json
    "templates": {
        "cleverLinks": false,
        "monospaceLinks": false,
        "default": {
            "outputSourceFiles": true
        },
        "systemName"      : "DocStrap",
        "footer"          : "",
        "copyright"       : "DocStrap Copyright © 2012-2013 The contributors to the JSDoc3 and DocStrap projects.",
        "navType"         : "vertical",
        "theme"           : "cerulean",
        "linenums"        : true,
        "collapseSymbols" : false,
        "inverseNav"      : true
    },
```

Then this is how you generate:

```bash
./node_modules/.bin/jsdoc yourJavaScriptFile.js -t ./node_modules/ink-docstrap/template -c ./node_modules/jsdoc/conf.json
```

And that's how you have a much better looking website.


## Using Grunt

The last command is long and tedious. Let' use Grunt to improve and automate the process.

Install [Grunt](http://gruntjs.com/):

```bash
npm install -g grunt-cli
```

Then install the plugin [Grunt-Jsdoc](https://npmjs.org/package/grunt-jsdoc):

```bash
npm install grunt-jsdoc --save-dev
```

Configure the Gruntfile and use [accordingly](https://npmjs.org/package/grunt-jsdoc). This is mine after configuring:

```js
module.exports = function(grunt) {

  // Project configuration.
  grunt.initConfig({
    jsdoc : {
        dist : {
            src: ['./*.js'],
            jsdoc: './node_modules/.bin/jsdoc',
            options: {
                destination: 'doc',
                configure: './node_modules/jsdoc/conf.json',
                template: './node_modules/ink-docstrap/template'
            }
        }
    }
  });

  grunt.loadNpmTasks('grunt-jsdoc');

};
```

Now, you can simple run `grunt jsdoc`.


## Others

There is a good review of [Jsdoc vs Docco vs Yuidoc vs Doxx](http://blog.fusioncharts.com/2013/12/jsdoc-vs-yuidoc-vs-doxx-vs-docco-choosing-a-javascript-documentation-generator/).

[Dox](https://github.com/visionmedia/dox) is another good one, but it now no longer spits out html. It only spits out JSON, yet there is no good render template with it.

[Smartcomments](http://smartcomments.github.io/) is helpful to generate `@param` etc, but it didn't work well for me. Not a big deal missing it.

Lastly, manual handwritten wiki style is still great. It is good for tutorials such as Express documentation. You can write in markdown or HTML.
