---
layout: post
title: "Migrating Octopress 2 to Octopress 3"
date: 2015-09-30T23:45:26+08:00
categories: [Octopress]
---
While [Octopress 3.0 announced it is coming soon](http://octopress.org/2015/01/15/octopress-3.0-is-coming/), it has been 9 months, but they _still_ have not release officially.

However, you can use [pre-released Octopress 3.0](https://github.com/octopress/octopress).

This post is a guide of my experience as I migrate this website from an existing Octopress 2.0 to the new Octopress 3.0 (with this [Medium lookalike theme](https://github.com/dirkfabisch/mediator)).

The older website has moved to [o2.samwize.com](http://o2.samwize.com).



## Installing

    mkdir mywebsite.com
    cd mywebsite.com
    touch Gemfile

Edit the Gemfile as follows:

```ruby
source 'https://rubygems.org'
 
gem 'jekyll'
gem 'github-pages'
gem 'bourbon'

group :jekyll_plugins do
    gem 'octopress', '~> 3.0.11'
    gem 'octopress-image-tag'
    gem 'octopress-video-tag'
    gem 'octopress-codeblock'
    gem 'octopress-quote-tag'
    gem 'octopress-codefence'
    gem 'octopress-solarized'
    gem 'octopress-gist'
end
```

Octopress 2.0 is a derivative of jekyll with more features. 

Octopress 3.0 is merely a plugin to jekyll -- which is a better design and plays well with the powerful jekyll. And there are other plugins provided by octopress such as `octopress-codefence` for [backtick code blocks](http://octopress.org/docs/plugins/backtick-codeblock/).

Run `bundle install` to install the gems.


    
## Setup a new website

    # In the directory mywebsite.com
    octopress new .

With that, you can now run the website locally: 
    
    bundle exec jekyll serve

Note: You need to prepend `bundle exec` so that the gems are used in this context.



## Migrating Your Posts and etc

Migrating octopress/jekyll is rather easy, because the your pages and posts are your markdown files.

- Copy all the markdown post (`/source/_posts` to `/_posts`)
- Copy all the markdown pages
- Copy all the images eg. (`/source/images` to `/images`)



## Plugins

To activate the [jekyll plugins](http://jekyllrb.com/docs/plugins/), edit `_config.yml` and add the gems:

    gems: 
      - octopress-image-tag
      - octopress-video-tag
      - octopress-codeblock
      - octopress-quote-tag
      - octopress-codefence
      - octopress-solarized
      - octopress-gist



## Themes

Jekyll themes are merely fork of Jekyll with custom html/css/etc.

For this website, I use [mediator](https://github.com/dirkfabisch/mediator).

Since we didn't start with a fork of a theme, we have to download/clone the theme, then copy (and replace) these:

- _config.yml
- _includes
- _layouts
- _sass
- assets
- css
- feed.xml
- index.html

You might also need the merge the theme's Gemfile to yours, if the theme uses other gems.



## Permalinks

The default jekyll permalink for each post is different from Octopress 2.0.

In Octopress 2.0, it is:

    http://samwize.com/2015/09/30/migrating-octopress-2-to-octopress-3/

In Octopress 3.0 (or Jekyll), the [default](http://jekyllrb.com/docs/permalinks/) is:

    http://samwize.com/some_category/2015/09/30/migrating-octopress-2-to-octopress-3.html

To change, edit `_config.yml` and add:

    permalink: /:year/:month/:day/:title/



## Github Pages

I host this website on Github Pages, for free. You can find out how to host it on [this post](http://samwize.com/2015/09/02/how-to-host-a-website-on-github-pages/).

    # To setup
    octopress deploy init git

Then edit `_deploy.yml` with your github repos.

You will also need to add a `CNAME` file with your domain name in it in `/mywebsite.com`.

    # Then push and deploy with
    octopress deploy



