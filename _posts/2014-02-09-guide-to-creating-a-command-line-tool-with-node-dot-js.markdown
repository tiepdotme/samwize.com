---
layout: post
title: "Guide to creating a command line tool with node.js"
date: 2014-02-09 18:28:23 +0800
comments: true
categories: Node
---

Ever wonder how after `npm install`, you could use the installed tool on the command line, as if they were added to your `bin`?

For example, after installing mocha, you could run mocha on your command line directly.

This is a guide on how to do that.

<!-- more -->

## Create the executable file

Firstly, say you have this `superbot.js` that uses [commander.js](https://github.com/visionmedia/commander.js/) that handles the command line options.

It is good practice to put such javascript in `bin` folder, so let's assume that.

The first line of the file should also have a _shebang_, that tells terminal to run with node.

    #!/usr/bin/env node

With that, you can run `./bin/superbot.js` on the command line, instead of `node ./bin/superbot.js`.

Also make sure the javascript is executable permission eg `chmod +x ./bin/superbot.js`.


## Add bin to package.json

Add a [bin key](https://npmjs.org/doc/json.html#bin) to `package.json` like this:

    "bin" : { "superbot" : "./bin/superbot.js" }

When someone does a `npm install`, npm will create a symlink from `./bin/superbot.js` to their `/usr/local/bin/superbot`.

That's why right after install, they can run commands such as:

    $ superbot --powerup somefile.txt

But if you are the developer, and you try to run any command, it will not work, yet. You need to run:

    npm link

This will explicitly create the symlink.

That's it!

PS: Another good (but longer) guide to build a CLI utility from [Jonathan Fielding](http://flippinawesome.org/2013/07/29/writing-a-command-line-utility-using-node/) using: grunt-init, commander.js and underscore.js .
