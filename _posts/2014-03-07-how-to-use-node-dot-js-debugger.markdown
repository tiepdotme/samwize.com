---
layout: post
title: "How to use Node.js Debugger"
date: 2014-03-07 14:50:41 +0800
comments: true
categories: [Node]
---

Node.js did come with a built-in [debugger](http://nodejs.org/api/debugger.html).

## How to use

<!-- more -->

### 1. Add breakpoint into code

To add a breakpoint, add this line of code:

    debugger;

Yup, just 1 word to break there.

### 2. Run in debug mode

Say the script you run is `app.js`, you will run `node` with a `debug`.

    node debug app.js

Your script will run and break on the line as specified.

For example:

    break in /some/path/app.js:1
      1 x = 5;
      2 setTimeout(function () {
      3   debugger;
    debug>


### 3. The commands to stepping around

At this point of time, you can issue the following commands to step around the code:

- `c` - Continue execution
- `n` - Step next
- `s` - Step in
- `o` - Step out
- `pause` - Pause running code

Or these commands for breakpoints:

- `sb()` - Set breakpoint on current line
- `sb(line)` - Set breakpoint on specific line
- `sb('fn()')` - Set breakpoint on a first statement in functions body
- `sb('script.js', 1)` - Set breakpoint on first line of script.js
- `cb` - Clear breakpoint

Or more info:

- `bt` - Print backtrace of current execution frame
- `list(5)` - List scripts source code with 5 line context (5 lines before and after)
- `watch(expr)` - Add expression to watch list
- `unwatch(expr)` - Remove expression from watch list
- `watchers` - List all watchers and their values (automatically listed on each breakpoint)


### Bonus: REPL

This is one of the best feature. It kinds of like code inspection in interative mode.

- `repl` - Open debugger's repl for evaluation in debugging script's context

Nice tool from node (even better than [Python](http://blog.just2us.com/2012/07/python-debugger-guide-pdb-kind-of-gdb/))!
