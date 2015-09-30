---
layout: post
title: "Homebrew broken on Yosemite"
date: 2015-01-29 18:06:56 +0800
comments: true
categories: 
---

After upgrading to Yosemite, your `brew` could run into an error:

    /usr/local/bin/brew: /usr/local/Library/brew.rb: /System/Library/Frameworks/Ruby.framework/Versions/1.8/usr/bin/ruby: bad interpreter: No such file or directory
    /usr/local/bin/brew: line 23: /usr/local/Library/brew.rb: Undefined error: 0

<!-- more -->

This is because the path to ruby has changed. It can be [solved](http://stackoverflow.com/a/24244945/242682) easily be editing `/usr/local/Library/brew.rb`.

Replace the first line "1.8" to "current" so it looks like this:

    #!/System/Library/Frameworks/Ruby.framework/Versions/Current/usr/bin/ruby -W0

As a side effect, you could run into errors with "files would be overwritten by merge" when you `brew update`.

To solve, you need to [update](http://stackoverflow.com/a/12031907/242682) the brew repos.
