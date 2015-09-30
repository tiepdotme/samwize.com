---
layout: post
title: "Cannot restart mysql for Digital Ocean server (low memory)"
date: 2014-06-10 14:07:52 +0800
comments: true
categories: [VPS]
---

I had an "error connecting to database" when accessing one of my wordpress website hosted on Digital Ocean.

    ps ax | grep mysqld

`mysqld` isn't a running process. Clearly mysql was stopped for some reason.

<!-- more -->

    cat /var/log/mysql/error.log

Checking the error log, it shows that there is a problem starting InnoDB. Seems like insufficient memory.

    140610  2:04:05 [Note] /usr/sbin/mysqld: Shutdown complete
    140610  2:04:06 InnoDB: Initializing buffer pool, size = 128.0M
    InnoDB: mmap(137363456 bytes) failed; errno 12
    140610  2:04:06 InnoDB: Completed initialization of buffer pool
    140610  2:04:06 InnoDB: Fatal error: cannot allocate memory for the buffer pool

Hence i check my available memory:

    free -m

Indeed, I have not much free memory to start mysql..

                 total       used       free     shared    buffers     cached
    Mem:           490        456         34          0          1         76
    -/+ buffers/cache:        378        111
    Swap:            0          0          0

To recover, I restart apache2 to free up some memory

    sudo service apache2 restart

And with that, my free memory increased from 34MB to 153MB, which is enough to restart mysql.

    sudo service mysql restart

Question now is:

> How to prevent this error again (when memory runs low)?


# Swap Memory

The tiny droplet I have has 512MB RAM, and 20GB on SSD harddisk.

It's a waste not to create swap memory out of the 20GB SSD!

So, [add a few GB of swap memory to your Ubuntu](https://www.digitalocean.com/community/tutorials/how-to-add-swap-on-ubuntu-14-04).

Steps:

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

That's it.

Without spending a cent, memory increased!
