---
layout: post
title: "New Theme for Octopress"
date: 2013-05-23 23:44
comments: true
categories: [Octopress]
---

I have changed the classic Octopress theme to this cleaner greyshade theme by [Shashank Mehta](http://shashankmehta.in/archive/2012/greyshade.html).

This is how you can install:

<!-- more -->

```
$ git submodule add git@github.com:shashankmehta/greyshade.git .themes/greyshade
$ git submodule update --init
$ echo "\$greyshade: #1DBE49;" >> sass/custom/_colors.scss
$ rake "install[greyshade]"
```

Note that the [only condition](https://github.com/shashankmehta/greyshade#readme) from the author is that you use a unique highlight color. In the above, my highlight color is `#1DBE49` (my kind of green).

After installing, I entered my `email` in `_config.yml` which is needed to show the profile pic (via Gravatar).

With that, here are screenshots of the old vs new:

{%img center /images/theme-classic.png %}

{%img center /images/theme-greyshade.png %}