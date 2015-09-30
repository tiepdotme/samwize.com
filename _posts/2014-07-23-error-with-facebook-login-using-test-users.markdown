---
layout: post
title: "Error with Facebook Login using test users"
date: 2014-07-23 13:18:22 +0800
comments: true
categories: [iOS, Parse]
---

I had encountered TWICE, of Apple reviewers rejecting 2 of my apps because while testing, they encountered an error with Facebook login.

The errors they might have seen are:

    com.facebook.sdk:SystemLoginCancelled

or

    The user is not allowed to see this application per the developer set configuration.

or

    Login failed with error code 800 error 800.

<!-- more -->

## What went wrong?

The above errors are most likely caused by using a **Facebook test user account** to log into a live (non-sandboxed) Facebook app.

Even Apple reviewers could make the mistake. 

They might have previously logged in a test account for another app, and while testing your app, they re-used the test account, accidentally.

It seems that Apple reviewers only do Facebook login via Safari means (not logged in via device Settings, nor via Facebook app). 

The solution is simply telling the reviewer that they might have to logout the current facebook account, and use a real account provided by you. Go to Safari > http://m.facebook.com > logout.
