---
layout: post
title: "Node.js request module with gzip/gunzip"
date: 2014-05-27 12:59:23 +0800
comments: true
categories: [Node]
---

Using [request module](https://github.com/mikeal/request) is the most popular way to get/post to a HTTP endpoint in Node.js.

A common question while using request is: How do I gunzip a HTTP response?

<!-- more -->

There are multiple solutions I have seen, and some didn't work for me.

There are some more advanced ones, [using streams](http://nickfishman.com/post/49533681471/nodejs-http-requests-with-gzip-deflate-compression).

Here's mine:

```js
var request = require('request');
var zlib = require('zlib');

var options = {
  url: 'http://some.endpoint.com/api/',
  headers: {
    'X-some-headers'  : 'Some headers',
    'Accept-Encoding' : 'gzip, deflate',
  },
  encoding: null
};

request.get(options, function (error, response, body) {

  if (!error && response.statusCode == 200) {
    // If response is gzip, unzip first
    var encoding = response.headers['content-encoding']
    if (encoding && encoding.indexOf('gzip') >= 0) {
      zlib.gunzip(body, function(err, dezipped) {
        var json_string = dezipped.toString('utf-8');
        var json = JSON.parse(json_string);
        // Process the json..
      });
    } else {
      // Response is not gzipped
    }
  }

});
```

The above will also fix for `Incorrect header check` [error](http://stackoverflow.com/q/19438884/242682), with `encoding: null` option.
