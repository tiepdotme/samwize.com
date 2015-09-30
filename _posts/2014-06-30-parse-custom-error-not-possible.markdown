---
layout: post
title: "Parse Custom Error Not Possible"
date: 2014-06-30 15:11:20 +0800
comments: true
categories: Parse
---

Once again, I am disappointed with Parse [official rep answers](https://www.parse.com/questions/custom-error-response-codes-for-cloud-code), which are, [again](/2014/05/30/parse-error-141-uncaught-userid-must-be-a-string/), wrong.

It is NOT possible to change the error code or message of a `response`. It will always be status code 141 (Parse Script Error) when you do a `response.error(...)`.

The best you can do is to pass a json as a message.
