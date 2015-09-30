---
layout: post
title: "Simple HTTP GET/POST Request in Node.js"
date: 2013-08-31 16:01
comments: true
categories: [Node]
---

This is a simple tutorial using mikeal's [super-simple-to-use request](https://github.com/mikeal/request) library.

<!-- more -->

If you don't know how to setup node.js libraries, read [this](/2013/08/29/getting-started-with-node-dot-js-scripting/).

This tutorial will provide sample codes for:

- setting the HTTP headers, 
- setting the URL query string for GET
- setting the HTTP body for POST
- handling gzip response
- handling json response


## GET ##

```js
var request = require('request');

// Set the headers
var headers = {
    'User-Agent':       'Super Agent/0.0.1',
    'Content-Type':     'application/x-www-form-urlencoded'
}

// Configure the request
var options = {
    url: 'http://samwize.com',
    method: 'GET',
    headers: headers,
    qs: {'key1': 'xxx', 'key2': 'yyy'}
}

// Start the request
request(options, function (error, response, body) {
    if (!error && response.statusCode == 200) {
        // Print out the response body
        console.log(body)
    }
})
```

Above code will do GET https://localhost:8080/?key1=xxx&key2=yyy


## POST ##

```js
var request = require('request');

// Set the headers
var headers = {
    'User-Agent':       'Super Agent/0.0.1',
    'Content-Type':     'application/x-www-form-urlencoded'
}

// Configure the request
var options = {
    url: 'http://samwize.com',
    method: 'POST',
    headers: headers,
    form: {'key1': 'xxx', 'key2': 'yyy'}
}

// Start the request
request(options, function (error, response, body) {
    if (!error && response.statusCode == 200) {
        // Print out the response body
        console.log(body)
    }
})
```

Above code will do POST https://localhost:8080/ with key1=xxx&key2=yyy in the body.


## Handling gzip encoding

To handle gzip encoded response, you will need the `compress-buffer` library.

```js
var uncompress = require('compress-buffer').uncompress;

// Set the headers and options ...

request(options, function (error, response, body) {
    if (!error && response.statusCode == 200) {
        // Handle gzip
        var encoding = response.headers['content-encoding']
        if(encoding && encoding.indexOf('gzip')>=0) {
            body = uncompress(body);
        }
        body = body.toString('utf-8');
        
        // Print out the response body
        console.log(body)

        // If it is json
        var json_body = JSON.parse(body);
    }
})
```
