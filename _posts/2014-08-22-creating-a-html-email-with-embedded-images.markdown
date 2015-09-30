---
layout: post
title: "Creating a HTML Email with embedded images"
date: 2014-08-22 11:16:14 +0800
comments: true
categories: [HTML]
---

Have you ever wondered how to send those nice emails with images embedded (and not as attachment)?

They are actually just HTML, with some pitfalls depending on how email apps render them.

In everyday emailing, no one bothers writing HTML. But I have my big day invitation to send, which I want to make it pretty. And so I went on to create my own HTML email.

Here's what I learnt.

<!-- more -->

There are a few ways to create the HTML Email:

- Use mailchimp's email builder
- Buy a template
- Write HTML from scratch

I choose to write my own HTML after some research. It turned out to be a good decision, and it isn't that difficult, if you already know basic HTML and CSS.

Let me show my final email screenshots before I explain on the pointers of creating the HTML email.

On a full blown web browser or email app:

{%img center /images/html-email-actual-full.jpg Actual Full Screen%}

On mobile (iPhone Mail app):

{%img center /images/html-email-actual-mobile.jpg Actual Mobile%}


# How much code?

For the record, to create the html email, I have only 75 lines of HTML code, and 3 images on my server.

So that's not too difficult.

I spent about 3 hours designing using Sketch app. The mockup produced was 90% similar to the deliverable. One big change is I replaced my custom font with web fonts. 

Another 3 hours was learning and writing the HTML code from scratch.


# Email Clients

Writing HTML for email is different from modern web development, where you can use CSS and Javascript. 

For Email, the platforms are even more fragmented than web browsers. There are web services such as yahoo.com, gmail.com and outlook.com. And there are lots of email clients such as outlook, lotus, airmail, etc..

Therefore, you have to use the older technology of HTML.


# Table, table, and table

Instead of using `<div>`, you have to use `<table>`, `<tr>` and `<td>`.

So design your layout with tables, within tables.. 

Don't rely on paddings, if you can use table.


# 600px Container

Your main content should be in a table no larger than 600px, because most email clients display under 600px.

# Fonts

You should use web fonts and [@import](https://www.campaignmonitor.com/blog/post/3897/using-web-fonts-in-email). Avoid `@font-face` because it is not well supported than `@import` method.

Search for a compatible font in [Google Web Fonts](http://www.google.com/fonts), and use theirs. 


# Images on a public server

When using any images, use absolute link to your public server.

That's also how they are not as an attachment in the email.


# Litmus Test

You can use [litmus.com](http://litmus.com) (free 14 days trial) to render your email on various devices.

It's bound not to work 100%, so don't fret. For some (old) email clients, they might not render correctly.

Missing images is common too because some email clients don't load images automatically for security reasons.


# Fallback Plan

And so, always have this line on the very top/bottom:

`If you're having difficulty viewing this email, please click here.`


# Reference

- [Coding HTML Email from scratch](http://webdesign.tutsplus.com/articles/build-an-html-email-template-from-scratch--webdesign-12770) by tutsplus

- [HTML Email in a nutshell](http://kb.mailchimp.com/article/how-to-code-html-emails/) by MailChimp
