---
layout: post
title: "youtube-dl Fights Back!"
date: 2020-11-17T09:39:37+08:00
categories: []
---

youtube-dl was taken down by DMCA a few months ago.

Their lawyers provided a [letter](https://github.com/github/dmca/blob/master/2020/11/2020-11-16-RIAA-reversal-effletter.pdf) on how they didn't violate any law. The key point is that **they didn't circumvent Youtube's technology**.

## What's YouTube's technology?

YouTube claims to have a _protection measure_ to prevent anyone from streaming the video.

But it is not hard to understand it, if you know how to look into the Javascript and the URL requests.

> To borrow an analogy from literature, travelers come upon a door that has writing in a foreign language. When translated, the writing says "say 'friend' and enter".
>
> The travelers say "friend and the door opens. As with the writing on that door, YouTube presents instructions on accessing video streams to everyone who comes asking for it."

In other words, YouTube didn't do enough to protect access to their video streams.

This is **unlike** hardcore protection like CSS encryption used on **DVD or Widevine DRM**.

## Definition of Circumvention

> youtube-dl does not "circumvent" it as that term is defined in Section 1201(a) of the Digital Millennium Copyright Act, because YouTube provides the means of accessing these video streams to anyone who requests them.

> As federal appeals court recently ruled, one does not "circumvent" an access control by using a publicly available password.

Digital Drilling Data Systems, L.L.C. v. Petrolink Services, 965 F.3d 365, 372 (5th Cir. 2020)

> **Circumvention is limited to actions** that "descramble, decrypt, avoid, bypass, remove, deactivate or impair a technological measure," without the authority of the copyright owner.

> What is missing from this statutory definition is any reference to 'use'

Egilman v. Keller & Heckman, LLP., 401 F. Supp. 2d 105, 113 (D.D.C. 2005)

Because youtube-dl is simply 'using' the "signature" code provided by YouTube in the same manner as any web browser -- it does not circumvent.

## Github backs youtube-dl

[Github](https://github.blog/2020-11-16-standing-up-for-developers-youtube-dl-is-back/) reinstate youtube-dl after receiving the EFF explanation letter.

Seems like they decided to support developers who are 'not circumventing' such weak protection measures.

They also find good use cases for youtube-dl:

> Although we did initially take the project down, we understand that just because code can be used to access copyrighted works doesn’t mean it can’t also be used to access works in non-infringing ways.

> We also understood that this project’s code has many legitimate purposes, including changing playback speeds for accessibility, preserving evidence in the fight for human rights, aiding journalists in fact-checking, and downloading Creative Commons-licensed or public domain videos.

Going forward, they are also going to improve their takedown process.

> Every single credible 1201 takedown claim will be reviewed by technical experts, including when appropriate independent specialists retained by GitHub, **to ensure that the project actually circumvents a technical protection measure** as described in the claim.

They also setting up a [Developer Defense Fund](https://github.blog/2020-11-16-standing-up-for-developers-youtube-dl-is-back/#developer-defense-fund) to help **face off aginst giant corporations**.

Nice.
