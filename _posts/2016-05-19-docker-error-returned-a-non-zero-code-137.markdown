---
layout: post
title: "Docker Error: Returned a Non-zero Code: 137"
date: 2016-05-19T10:40:29+08:00
categories: [Docker]
---

While running a node app with Docker, there is an error 137:

    The command '/bin/sh -c npm install' returned a non-zero code: 137

This means a [out of memory error](https://github.com/mitchellh/vagrant/issues/4247).

To fix, you can add more RAM.

Or you can add more swap memory (FREE!). Swap memory uses part of your harddisk for temporary memory.

These steps are exactly the same from a [previous guide](/2014/06/10/cannot-restart-mysql-for-digital-ocean-server-low-memory/):

```bash
# Confirm you have no swap
sudo swapon -s

# Allocate 1GB (or more if you wish) in /swapfile
sudo fallocate -l 1G /swapfile

# Make it secure
sudo chmod 600 /swapfile
ls -lh /swapfile

# Activate it
sudo mkswap /swapfile
sudo swapon /swapfile

# Confirm again there's indeed more memory now
free -m
sudo swapon -s

# Configure fstab to use swap when instance restart
sudo nano /etc/fstab

# Add this line to /etc/fstab, save and exit
/swapfile   none    swap    sw    0   0

# Change swappiness to 10, so that swap is used only when 10% RAM is unused
# The default is too high at 60
echo 10 | sudo tee /proc/sys/vm/swappiness
echo vm.swappiness = 10 | sudo tee -a /etc/sysctl.conf
```
