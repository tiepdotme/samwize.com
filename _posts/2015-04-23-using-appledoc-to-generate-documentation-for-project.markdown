---
layout: post
title: "Using appledoc to generate documentation for project"
date: 2015-04-23 11:00:34 +0800
comments: true
categories: [iOS]
---

It is good practise to generate documentation for your project.

[appledoc](https://github.com/tomaz/appledoc) is a tool to help generating the HTML docs from your code.

This is a guide on how to do it.

<!-- more -->

## Install VVDocumenter-Xcode

Firstly, install [this Xcode plugin](https://github.com/onevcat/VVDocumenter-Xcode) to help you with generating Javadoc style by simply typing `///` (3 forward slashes).

    git clone https://github.com/onevcat/VVDocumenter-Xcode.git

Open the project and build.

Restart Xcode.

Typing `///` above a method should now generate the template for you to document on a method.



## Install appledoc

Install [appledoc](https://github.com/tomaz/appledoc) and build it.

    git clone https://github.com/tomaz/appledoc.git
    sudo sh install-appledoc.sh

Note: `brew install appledoc` doesn't work with Yosemite as of April 2015, so git clone and build is the way to go.



## Create a AppledocSettings.plist

You can refer to a [basic use of appledoc](https://github.com/tomaz/appledoc/wiki/appledoc-docs-examples-basic) at this point, but a basic use would have a number of parameters to the command.

It is better to create a plist to contain the parameters.

The default plist for a project is `AppledocSettings.plist`.

The plist for my project looks like this:

```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN"
        "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
    <plist version="1.0">
    <dict>
        <key>--project-name</key>
        <string>Jade</string>
        <key>--project-company</key>
        <string>Just2us</string>
        <key>--company-id</key>
        <string>com.justus</string>

        <key>--output</key>
        <string>Docs</string>

        <key>--create-docset</key>
        <false/>

        <key>--ignore</key>
        <array>
            <string>Pods</string>
            <string>Libs</string>
            <string>JadeTests</string>
            <string>*.framework</string>
        </array>

    </dict>
    </plist>
```

I have some basic information about the project and the company. Then I am generating the HTML in `Docs`, without docset, and ignoring some folders such as Pods, tests and frameworks.

With that, in your project directory, run

    appledoc .

That's it!

The HTML will be in `Docs`.

## Advanced

There are some [advanced commands](https://github.com/tomaz/appledoc/wiki/appledoc-docs-examples-advanced) and good guide about publishing from [Cocoanetics](http://www.cocoanetics.com/2011/11/amazing-apple-like-documentation/).
