---
layout: post
title: "Octopress Table Stylesheet"
date: 2012-09-24 19:27
comments: true
categories: Octopress
published: true
---

Octopress has been very cool for the 2 weeks since I began using. 

My first hiccup came when I tried using table in [this post](/2012/09/21/i-bought-samwize-dot-com-for-99-cents/). Firstly, I have to figure out how to create table in Octopress. Yet after figuring out, the table doesn't get displayed! It's just not working.

<!-- more -->

## Usage ##

The first problem is with understanding how to create table in Octopress. They didn't really [document](http://octopress.org/docs/) that, because they expect you to know [markdown inline HTML](http://daringfireball.net/projects/markdown/syntax#html) can be used. That means writing:

``` html
<table>
    <tr>
        <td>Column1</td>
        <td>Column2</td>
    </tr>
    <tr>
        <td>foo</td>
        <td>foo</td>
    </tr>
</table>	
```

However, that's not really nice, and defeats the point of _not writing in HTML_. 

Fortunately, you can do the same using extended markdown syntax like this:

```
| Column1     | Column2      |
| ----------- | ------------ |
| foo         | foo

```

But that still does not work. You will not see the table borders..

I would say that's a bug with Octopress.

## Table Stylesheet ##

As I inspect the generated HTML, the table tags are present, and correct. 

They are not showing because of the css stylesheet. `table`, `th` and `td` has `border-width` of ZERO! This [post in chinese](http://programus.github.com/blog/2012/03/07/add-table-data-css-for-octopress/) provides a solution. Translated, it means:

### Step 1. Add data-table.css ###

Add [data-table.css](https://gist.github.com/programus/1993032#file-data-table-css) to `source/stylesheets/`.

{% gist 1993032 data-table.css %}

### Step 2. Add link header ###

In `source/_includes/head.html` insert this: 

``` html
<link href="{{ root_url }}/stylesheets/data-table.css" media="screen, projection" rel="stylesheet" type="text/css" />
```

And now the table appears!

PS: I am puzzled why Octopress does not ship with a default table stylesheet.

 

