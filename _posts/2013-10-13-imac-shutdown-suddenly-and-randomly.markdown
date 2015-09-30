---
layout: post
title: "iMac Shutdown Suddenly and Randomly"
date: 2013-10-13 15:29
comments: true
categories: Mac
---

I have recently faced a critical issue with my iMac (early 2013 model).

It crashes/shutdown randomly and suddenly. It's as if the power cord has been disconnected.

And it doesn't restart when I press the power on button. I have to switch off the power socket, wait for a few minutes, then it can be turned back on. Sometimes, if I try to turn back on too early, I can notice my optical mouse lighted up briefly, and the iMac still doesn't start. These symptons make me wonder is it a power issue..

These were some thing I did.. which still have not solve the issue..

If you know something else that I should do, I will greatly appreciate!

<!-- more -->


# 1. Console says Cause 0

I checked Console.app, and it logged:

	kernel: Previous Shutdown Cause: 0

It was said on the [forum](https://discussions.apple.com/thread/3128527?start=0&tstart=0) that a Cause 0 means the power was disconnected. This gives me some clue how it gets shut off.

It will be Casue 5 if triggered from the menu.


# 2. Running Apple Hardware Test

To isolate that the problem, I tried running [AHT (Apple Hardware Test)](http://support.apple.com/kb/ht1509).

You can press and hold D while booting to perform the test.

But the test said the hardware is fine. But I can't be 100% sure. In fact I am thinking it could be caused by the SSD or RAM.


# 3. Installing Temperature Monitor

I installed [Temperature Monitor app](http://www.macupdate.com/info.php/id/12381/temperature-monitor) to log the temperature of the CPU, GPU, etc.

That again didn't shed light on the cause. The temperatures were all okay when the system crashed.



# 4. Learnt about resetting SMC

I learnt that what I have been doing - pulling off the power plug - is the way to [reset System Management Controller (SMC)](http://support.apple.com/kb/HT3964). 

For my case, it seems a bit different. I need to power off for 1 minute instead of 20 seconds.


# 5. Run memtest

Suspecting it could be a RAM that turned faulty, I run [memtest](http://osxdaily.com/2011/05/03/memtest-mac-ram-test/). That is basically a command line tools that write random data to all of the RAM and verify it is correct.

I run `memtest all 2` and nothing bad turned up.


# 6. PRAM Reset

Finally, I called Apple support for advise. One thing new they asked me to perform is the [PRAM reset](http://support.apple.com/kb/HT1379).

During boot up, press and hold Command + Option + P + R. The computer will restart and when you hear the startup sound the 2nd time, you may release.

One thing weird is that after I hear the startup sound the 2nd time, the Mac goes blank. By right it should continue to boot up, but it didn't. In fact, it shutted down..

The Apple support deduce that the SMC logic board might have a problem and need to be replaced. 

Hence, I got to bring the Mac down to a service center..


# 7. Apple forum

[Apple forum](https://discussions.apple.com/message/23389173#23389173) didn't provide a clear resolution. At least I knew I am not the only one affected.


# 8. Sending in for repair

Alas, I carried the giant iMac down to a service center for repair. This is my most dreaded.

It took 1 week for the repair. They replaced the logic board (aka monther board).

That ends the saga. 

