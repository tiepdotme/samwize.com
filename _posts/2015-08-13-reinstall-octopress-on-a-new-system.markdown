---
layout: post
title: "Reinstall Octopress on a new system"
date: 2015-08-13 14:57:05 +0800
comments: true
categories: [Octopress]
---

Instructions for reinstalling an existing Octopress on a new system, using this website (samwize.com) as an example:

<!-- more -->

    git clone https://github.com/samwize/samwize.github.com samwize.com
    cd samwize.com
    git checkout source
    gem install bundler
    # Restore the themes
    git submodule update --init
    cd .themes/greyshade
    # Ensure custom remote https://github.com/samwize/greyshade is added
    git checkout -b master2 samwize/master2

