---
layout: post
title: "armv7s build for tesseract and opencv2"
date: 2013-06-08 00:07
comments: true
categories: iOS
---

Tesseract is the OCR engine that helps to recoginze text. Google now maintains [the project](https://code.google.com/p/tesseract-ocr/wiki/ReadMe).

You can use Tesseract on iOS too.

I [forked](https://github.com/samwize/iPhone-OCR-Tesseract-and-OpenCV) the github project because the [original](https://github.com/pablosproject/iPhone-OCR-Tesseract-and-OpenCV) has a couple of problems compiling. To be precise:

	File is universal (three slices), but it does not contain a(n) ARMv7-s slice for ..

<!-- more -->

This is because the original project was 10 months ago, and did not support for armv7s (iPhone 5). You can [hack](http://www.galloway.me.uk/2012/09/hacking-up-an-armv7s-library/) a build by yourself.

But I have fixed that for both tesseract libs and also updated opencv2 libs. 

For your convenience: [https://github.com/samwize/iPhone-OCR-Tesseract-and-OpenCV](https://github.com/samwize/iPhone-OCR-Tesseract-and-OpenCV)
