---
layout: post
title: "How to Renew Cert for Fastlane Match"
date: 2019-05-02T14:31:00+08:00
categories: [fastlane]
---

[Fastlane match](https://docs.fastlane.tools/actions/match/) is a great tool, except that they are super slow in fixing and improving, coincidentally [after the founder left](https://krausefx.com/blog/ending-my-fastlane-chapter).

I am pretty sadden that their solution to "fixing" every issue is:

![](/images/fastlane-bot-fix.png)

## Problem: Expired Certificates

A certificate last for 1 year.

When it expires, fastlane match does not auto-renew, and simply throw an error.

> [!] Your certificate '54V6LXXXXX.cer' is not valid, please check end date and renew it if necessary

This issue was [raised](https://github.com/fastlane/fastlane/issues/11663) more than a year ago, and then closed by the bot, without a fix.

Dang.

## Solution 1: nuke

You can use their `nuke` to **revoke your certificates and provisioning profiles**.

    fastlane match nuke development
    fastlane match nuke distribution
    fastlane match nuke enterprise

`nuke` will delete all. You cannot choose a particular cert or application identifier to kill.

Like a nuclear bomb, it is pretty destructive.

## Solution 2: Remove the certs in repo

There is another solution that is manual, but you can be selective.

For example, I can renew my development certificate (keeping the distribution), or remove only certain provisioning profiles, or affect only a certain application.

To do so, simply removing the files in the repository accordingly.

- /certs/development/54V6LXXXXX.p12
- /certs/development/54V6LXXXXX.cer
- /profiles/development/whichever.mobileprovision

Then do **git commit and push**.

You should also go to Apple Developer portal and delete the provisioning profiles, otherwise you will end up with profile names with random digits as suffix (fastlane added to avoid name collision).

When you run `fastlane match` again, it will re-create new cert and profiles.
