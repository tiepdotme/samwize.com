---
layout: post
title: "Increase Ubuntu Swap Space"
date: 2014-07-25 17:42:07 +0800
comments: true
categories: [VPS]
---

I regretted that I [initially](/2014/06/10/cannot-restart-mysql-for-digital-ocean-server-low-memory/) configured my swap space to just 1GB.

I am running out of memory with intensive apps such as [GitLab](/2014/07/24/how-to-setup-git-server-gitlab-with-ssl/). 

And so I needed to increase swap.

<!-- more -->

It is rather simple, and very similar to the [original post on adding swap memory](/2014/06/10/cannot-restart-mysql-for-digital-ocean-server-low-memory/).

Steps:

```
# Assume I need another 2GB
# Allocate 2GB in another file. Let's call it swapfile2g.
sudo fallocate -l 2G /swapfile2g

# Make it secure
sudo chmod 600 /swapfile2g

# Activate it
sudo mkswap /swapfile2g
sudo swapon /swapfile2g

# Confirm again there's indeed more memory now
free -m
sudo swapon -s

# Configure fstab to use swap when instance restart
sudo nano /etc/fstab

# Add this line to /etc/fstab, save and exit
/swapfile2g   none    swap    sw    0   0
```
