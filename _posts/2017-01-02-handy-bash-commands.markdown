---
layout: post
title: "Handy Bash Commands"
date: 2017-01-02T12:30:28+08:00
categories: [bash]
---

## Resize Image

    # Resize to max width/height 640
    sips -Z 640 *.jpg

## Convert Image Format

    # Convert png to jpg
    sips -s format jpeg *.png --out mydirectory

## List Process Running on a Port

    # eg. Port 8080
    lsof -i :8080

## Kill Process running on a Port

    lsof -P | grep ':8080' | awk '{print $2}' | xargs kill -9

## Creating Markdown Journal

Uses [journal](https://github.com/samwize/journal/):

    journal new -d /path/to/journal "My entry for today"

## Extract MP3 from Youtube

Uses [youtube-dl](https://rg3.github.io/youtube-dl/):

    youtube-dl -x --audio-format=mp3 https://www.youtube.com/watch?v=eu-5mvCNKbQ

    # Other handy youtube-dl
    # Download in mp4 video format
    youtube-dl -f mp4 https://www.youtube.com/watch?v=eu-5mvCNKbQ

## Download Youtube Live Stream

For live streams, `youtube-dl` does not work well. We need another tool to the rescue - ~~[livestreamer](https://github.com/chrippa/livestreamer/)~~ [streamlink](https://github.com/streamlink/streamlink)

    streamlink --hls-live-restart -o Live.ts https://www.youtube.com/watch?v=zkrq7Kpd1so best

## Download a file with curl

    curl -o myfile.mp3 https://the.domain.com/file

## Rename multiple files in a directory

    # Add a prefix "XXX_" to every file
    for f in * ; do mv "$f" "XXX_$f" ; done

## Search/grep

    # Find recursively in the directory for the string 'needle'
    grep -R 'needle' path/to/dir/
