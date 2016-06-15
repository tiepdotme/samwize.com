---
layout: post
title: "Swift Script to Download All WWDC 2016 Videos and PDFs Automatically"
date: 2016-06-15T11:11:10-07:00
categories: [Swift]
---

While I may be at WWDC physically (for the first time!), I still prefer to download and comfortably watch the sessions on my MacBook.

It is refreshing to be live and watching the presenter, and _excessively applause every now and then_. 

But it does not value add much.

A more efficient way is to skim through the PDF first, then watch the interested video at 1.5x speed (:


## My Swift Script

For [WWDC 2014](/2015/06/14/how-to-download-all-wwdc-2015-pdf-slides-only/) and [WWDC 2015](/2015/06/14/how-to-download-all-wwdc-2015-pdf-slides-only/), I had used [ohoachuck's wwdc-downloader](https://github.com/ohoachuck/wwdc-downloader). It was good, but it wasn't updated for 2016, yet.

Not too long ago I tried to [write more scripts with Swift](/2016/05/20/introduction-to-scripting-in-swift/).

So I thought I write in Swift to download the session videos and PDFs automatically! 

Go check out [wwdc-dl](https://github.com/samwize/wwdc-dl).

To use:

    # Download the PDFs first
    ./wwdc-dl.swift -s 205,207,208,204,206,301,302 --pdfonly
    
    # Download the interested videos (default SD)
    ./wwdc-dl.swift -s 205,302
    
And they will be in `~/Documents/WWDC-2016`.

_Note: It was hacked quickly at 4am, and it is plain simple, without progress indication or any error handling eg. If there is no PDF for the session, it will just crash!_

![Hello WWDC 2016!](/images/junda-wwdc-2016.jpg)
