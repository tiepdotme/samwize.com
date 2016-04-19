---
layout: post
title: "Have Fun With Git Lol Commits"
date: 2016-02-22T09:45:43+08:00
categories: [git]
---

[lolcommits](https://github.com/mroth/lolcommits) is a fun library that takes a photo/gif of you whenever you make a commit.

    # Install
    brew install imagemagick
    gem install lolcommits

    # To enable for a repository
    # cd to repos
    lolcommits --enable --delay=3

Note: Adding a delay helps to solve the problem of your webcam/isight camera adjusting to focus. If you didn't add a delay, you could see an all black photo!

The photo/gif are stored in `~/.lolcommits/yourproject/`.

## GIF

    # To take gif, you need to install ffmpeg
    brew install ffmpeg --with-fdk-aac --with-ffplay --with-freetype --with-frei0r --with-libass --with-libvo-aacenc --with-libvorbis --with-libvpx --with-opencore-amr --with-openjpeg --with-opus --with-rtmpdump --with-schroedinger --with-speex --with-theora --with-tools

    #Then enable to animate (eg for 1 sec)
    lolcommits --enable --delay=3 --animate=1


## Enable for all projects

For every project, you need to run `lolcommits --enable` so that it create a post commit hook.

But you could [enable for all repos](https://github.com/mroth/lolcommits/wiki/Enabling-Lolcommits-for-all-your-Git-Repositories):

    mkdir -p ~/.git_template/hooks
    git config --global init.templatedir '~/.git_template'
    cp [path/to/lolcommit-enabled-repo]/.git/hooks/post-commit ~/.git_template/hooks/

Now, if you want to enable lolcommits for an existing repos, run `git init`.

For new repos, when you run `git init`, it will automatically get enabled.


## Create timelapse movie

    convert `find . -type f -name "*.jpg" -print0 | xargs -0 ls -tlr | awk '{print $9}'` timelapse.mpeg
    