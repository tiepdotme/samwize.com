# README

## Setup

    bundle install


## Deploy on local

    bundle exec jekyll serve

Note: Can't just run `jekyll serve` because it needs the bundle/gem as context


## Deploy to Github Pages

    bundle exec octopress deploy

## Other commands

    # New post
    octopress new post "Post title"

    # New page
    octopress new page about.md
    # Or in a directory
    octopress new page _foo/bar.md

## CNAME

The CNAME file containing `samwize.com` MUST be manually put in `/_site`. 

Octopress/Jekyll should support this ? Just make sure after deploy it is still there.

