---
layout: post
title: "How to use Jade templating in Express"
date: 2013-10-20 12:32
comments: true
categories: [Node]
---

This is a tutorial for the bare minimum to use Jade templating in Express.

If you have already created a [basic](http://expressjs.com/guide.html) Express app, this is all you need to use Jade.

<!-- more -->

## 1. Create the views directory

Create a `views` directory in your project root directory. 


## 2. Configure the view engine

You can actually use other directory names instead of `views`, as you will configure in your app.js as such:

```js
app.configure(function(){
    app.set('views', __dirname + '/views');
    app.set('view engine', 'jade');
});
```


## 3. Write some jade code

Create `index.jade` in the `views` directory. 

A basic example:

```css
div.content
	p Hello Worls
```


## 4. Write the route

In the usual `app.get(..)` code, you can now render using the jade template.

```
app.get('/', function(request, response) {
    response.render('index.jade');
});
```

That's it!