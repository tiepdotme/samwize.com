---
layout: post
title: "Error: Address/Port Already in Use"
date: 2016-11-07T12:36:19+08:00
categories: [Pitfalls]
---

I have encountered many times trying to run a service that use an existing port.

For example, my `jekyll serve`:

    jekyll 3.1.6 | Error:  Ad dress already in use - bind(2) for 127.0.0.1:4000
    
This is because my jekyll process somehow wasn't killed properly, and then I tried to run another process using the same port.

To solve, kill the process that is using the port (eg 4000):

    lsof -i :4000
    kill -9 <PID>
