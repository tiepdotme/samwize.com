---
layout: post
title: "Schedule Cron Jobs on Mac With Crontab"
date: 2018-07-07T04:42:30+08:00
categories: []
---

I introduced `sleep` and `gtimeout` in my post on [some nice bash commands](/2017/01/02/handy-bash-commands/) available in macOS.

The advanced way of scheduling job is to run cron jobs.

[Crontab](http://www.adminschoice.com/crontab-quick-reference) sets up cron jobs and runs them at specific time, and repeatedly.

One frequent use case I have is to schedule my computer to download live youtube streams at wee hours, using [`streamlink`](https://github.com/streamlink/streamlink).

## Basic Cron Job

To edit the cron jobs, run:

    env EDITOR=nano crontab -e

We use nano text editor as that is easier. You need to save and exit the editor every time you finish editing it.

Enter a line of text, representing a cron job, like this:

    59 23 * * * cd ~/Documents && /usr/local/bin/streamlink https://www.youtube.com/watch\?v\=y3oW28Draso best >/tmp/stdout.log 2>/tmp/stderr.log

This long line:

- runs the command on 23:59 every day
- `cd` to a directory
- then run the `streamlink` command, with full path to the command because cron is run without knowing the env var `PATH`
- writes the output to /tmp/stdout.log and error to /tmp/stderr.log

## Use a script

To keep things tidy in crontab, I will often write the commands in a script eg "job.sh" in ~/Documents.

    cd ~/Documents
    touch job.sh
    chmod +x job.sh

The content of "job.sh":

```bash
#!/bin/sh

PATH=/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/go/bin

cd ~/Documents

now=`date "+%Y-%m-%d %H%M"`

streamlink -o "$now.ts" https://www.youtube.com/watch\?v\=9kVxPmh8eSQ best
```

- set the `PATH` env var, so that all common executables are available
- use `date` for the file name eg. "2018-07-07 0810.ts"
- calls `streamlink` to download live youtube stream

Then in crontab, each job is simplified to just running the script.

    59 23 * * * ~/Documents/job.sh >/tmp/stdout.log 2>/tmp/stderr.log

## Other Tips

    # List the processes with name streamlink
    ps aux | grep streamlink

    # Kill the process with the id eg. 26959
    kill -9 26959

    # Kill all streamlink processes in 1 command:
    ps aux | grep streamlink | awk '{print $2}' | xargs kill -9
