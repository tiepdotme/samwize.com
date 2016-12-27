---
layout: post
title: "Handy Bash Commands"
date: 2016-12-27T12:30:28+08:00
categories: [bash]
---

## Resize Image

    # Resize to max width/height 640
    sips -Z 640 *.jpg

## List Process Running on a Port

    # List all process running on port 8080
    lsof -i :8080

## Kill Process running on a Port

    # Kill process running on port 8080
    lsof -P | grep ':8080' | awk '{print $2}' | xargs kill -9

## Creating Markdown Journal

Uses [journal](https://github.com/samwize/journal/):

    journal new -d /path/to/journal "My entry for today"

## Extract MP3 from Youtube

Uses [youtube-dl](https://rg3.github.io/youtube-dl/):

    youtube-dl -x --audio-format=mp3 https://www.youtube.com/watch?v=eu-5mvCNKbQ
