---
layout: post
title: "Which Ad Network to use in iOS?"
date: 2012-11-13 21:24
comments: true
categories: iOS
---

My experience with using ads in iOS goes as far back in 2008 when iOS is released. It was simple back then, but now, I have lost count of the number of ad networks.

See what I mean:

[{%img center http://mobyaff2.danharrison.netdna-cdn.com/wp-content/uploads/2012/10/Mobile-Advertising-Networks-Market-Map-600.png %}](http://www.mobyaffiliates.com/blog/mobile-advertising-networks-market-map/)

This post I am going rave a bit. So..

tldr;

- Use 2 mediation - [Admob](http://admob.com) & [MoPub](http://mopub.com) - and split them 50-50
- Use [Admob](http://admob.com)
- Use [MillenialMedia/mMedia](http://mmedia.com/)
- Use [InMobi](inmobi.com)
- Use [iAd](https://developer.apple.com/iad/)

<!-- more -->

## In the early days.. ##

Back in the early days (2008), there is an obvious choice - [Admob](http://admob.com).

Back then, Admob is a startup, with strength in mobile analytics and mobile advertising. In fact, I started using them for mobile analytics in 2007. 

When iPhone boomed, so did their value as they have a strong mobile ad network. They were eventually acquired by Google in the range of $BILLION$!


## Then come mediation - Adwhirl ##

I was quite comfortable just serving ads from admob. It was also the first time that I had revenue coming from ads!

It was good, but I soon realized it could be better in around 2009.

Ads mediation became popular, and [Adwhirl](https://www.adwhirl.com/) was the popular choice, as they are free, uncluttered, and open source. 

By using a mediation means I start to use more than 1 ad network. Read on what I used.

Once again, Adwhirl was eventually acquired by Google..


## More ad networks ##

[Google Adsense](https://www.google.com/adsense/) was the main reason that I used a mediation. In 2009, Google was a newcomer to mobile advertising, with Admob dominating.

However, we all know Google has the resource to catch, plus it has good network on the web advertising. I applied to be the BETA to mobile Adsense. I got in, integrated with Adwhirl, and revenue gets even better!

I realized mediation does help a lot. Reasons:

- When 1 ad network cannot be filled, it can try others
- Inventory increased, so there is more varierty of ads shown

I also integrated Quattro and Greystripe, but with little success from them.

And also iAd, but little success as my users are mainly outside US.


## The Google world is not good ##

I have thought with the acquisition of Admob and Adwhirl, Google will provide great stuff.

I was totally wrong.

Admob is not improving. Their website design remains the same as 5 years ago. What's worse is that by introducing mediation and house ads, they now look complicated.

Adwhirl is deprecated. It's last SDK update is in 2011. If you want to use Adwhirl, you should use Admob. But there are actually better alternatives.


## MoPub - the new kid ##

I started looking into [MoPub](http://mopub.com) in 2012 for these reasons:

- Admob mediation is cranky. It reported fill rate as [low](https://groups.google.com/forum/#!msg/google-admob-ads-sdk/wLMKhV2pMiA/tHfttXGfzlEJ) as 4% for my other networks! (serving Admob ads is very fine >90%)

- Admob provides no support. Just like other Google service, it is hard to even find their support email. I posted on their Forum, but no Admob staff is replying.

- Admob SDK will crash when they serve InMobi (hence I cannot use InMobi at all)

- Admob website looks like in the 1990s

To sum up, I wanted to move away from Admob.

MoPub looks great, and has it's SDK opensourced. 

One of the problem with mediation is that integration is complex, and the network adaptors could be buggy (like the InMobi case with Admob). I thought MoPub is smart to provide their SDK source code. If there is a problem, at least we can fix it.

MoPub also promised integration with certain ad network without the need to integrate into the app. I have yet to try.


## The 50-50 mode ##

However, that said, I choose to use both Admob and MoPub as mediation. 

When I was using MoPub, I encountered some problems too.

- Frequently, it has the error "server response indicated no ad available." and doesn't get filled!

- MoPub serve in a eCPM waterfall manner. That is it get ads from the network that pays best first, if not filled, then it moves on.

Because of that, MoPub often serve the same ad. I seldom see Admob/Adsense ads. I rather have variety in my inventory.

To play safe, I do a 50-50 split.



