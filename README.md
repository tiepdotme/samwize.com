# README

## Setup

    bundle install


## Deploy on local

    bundle exec jekyll serve

Note: Can't just run `jekyll serve` because it needs the bundle/gem as context


## Deploy to Github Pages

    octopress deploy

## Other commands

    # New post
    bundle exec octopress new post "Post title"

    # New page
    bundle exec octopress new page about.md
    # Or in a directory
    bundle exec octopress new page _foo/bar.md

