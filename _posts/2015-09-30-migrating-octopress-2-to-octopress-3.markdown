---
layout: post
title: "Migrating Octopress 2 to Octopress 3"
date: 2015-09-30T23:45:26+08:00
categories: [Octopress]
---

[Octopress 3.0 is coming](http://octopress.org/2015/01/15/octopress-3.0-is-coming/). It has been 9 months, but they have not officially released it yet.

But you [can](https://www.justinrummel.com/migrating-from-octopress-2-to-octopress-3/) use [pre-released Octopress 3.0](https://github.com/octopress/octopress).

<!-- more -->

## Installing

    mkdir Octopress3
    cd Octopress3
    touch Gemfile

Edit the Gemfile as follows:

```ruby
    source 'https://rubygems.org'
     
    gem 'jekyll'
     
    group :jekyll_plugins do
        gem 'octopress', '~> 3.0.11'
        gem 'octopress-image-tag'
        gem 'octopress-video-tag'
        gem 'octopress-codeblock'
        gem 'octopress-quote-tag'
        gem 'octopress-codefence'
    end
```

In Octopress 3.0, octopress itself is a plugin to jekyll (all along, Octopress 2.0 is a jekyll with more features). There are other plugins for octopress such as `codefence` for [backtick code blocks](http://octopress.org/docs/plugins/backtick-codeblock/).

Run `bundle install`.

Setup a new site:

    octopress new mywebsite.com

You can now run: 
    
    cd mywebsite.co
    jekyll s


## Plugins

To activate the [jekyll plugins](http://jekyllrb.com/docs/plugins/), edit `_config.yml` and add this line:

    gems: [octopress-image-tag, octopress-video-tag, octopress-codeblock, octopress-quote-tag, octopress-codefence]



## Migrating Your Posts

Copy all the markdown posts in `_posts` from your old Octopress to this new Octopress.

That's about it for migration.


## Commands

    # To run locally on http://localhost:4000/
    jekyll s

    # To create new post
    octopress new post "Awesome Tittle"


## Themes

