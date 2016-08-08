---
layout: post
title: "Updating Jekyll to 3.0"
date: 2016-08-08T21:28:34+08:00
categories: [Jekyll]
---

This blog was running on Jekyll 2.4.0 since the day I [migrated away](/2015/09/30/migrating-octopress-2-to-octopress-3/) from Wordpress.

Today, I updated Jekyll to 3.1.6, primarily because I want to use kramdown, instead of redcarpet.

Starting from [Jekyll 3.0](https://github.com/blog/2100-github-pages-now-faster-and-simpler-with-jekyll-3-0), kramdown is the default. It is acknowledged to be the de facto markdown library, along with Rouge over Pygments.

This is a post on how to update Jeykll, with fix to make things working.


## Update Gemfile for Octopress

Update `Gemfile` for the octopress group. Many plugins are no longer necessary (at least for me) - codefence, video tag, quote tag, gist. I have in fact try to not to use octopress plugins, as they are not true markdown.

The last 2 gems for octopress hooks and paginate are necessary.

```ruby
group :jekyll_plugins do
  gem 'octopress'
  gem 'octopress-image-tag'
  gem 'octopress-solarized', :git => 'https://github.com/samwize/solarized'
  gem 'octopress-hooks', git: 'https://github.com/octopress/hooks.git'
  gem 'octopress-paginate'
end
```


## Bundle Update

With that, update the gems with

    bundle update

If you have no error, then good luck!

I wasn't in such luck. The first hurdle is to do with the terrible nokogiri. I have to [install](http://stackoverflow.com/a/19807558/242682) with this for El Capitan:

    gem install nokogiri -- --with-xml2-include=/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX10.11.sdk/usr/include/libxml2 --use-system-libraries

But should use this for future-proof:

    gem install nokogiri -- --with-xml2-include=`xcrun --show-sdk-path`/usr/include/libxml2 --use-system-libraries
    

## Building 

Once the gems are installed, edit `_config.yml`.

Specifically, REMOVE the markdown and highlighter. 

You should not specify kramdown or pygments, because they have been superseded by the better defaults.

Build with `jekyll build`.





### Error: Octopress Hooks & Paginate

If you follow my Gemfile, then you should not have the [error](https://github.com/octopress/paginate/issues/19): 

    Liquid Exception: undefined method `start_with?' for nil:NilClass in ...
    
### Error: Liquid


There are liquid parsing exception in some my posts like this:

    {% raw %}Liquid Exception: Variable '{{..' was not properly terminated with regexp{% endraw %} 

{% raw %}This is because I have used `{{`, which are marker for liquid. {% endraw %} 

To fix, you need to surround with the [raw tags](https://github.com/imathis/octopress/issues/466).

