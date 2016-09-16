---
layout: post
title: "Setup Your Own $5 VPN With Docker, OpenVPN and Digital Ocean"
date: 2016-09-10T22:09:52+08:00
categories: [VPS]
---

I have set up VPN on my virtual private servers before, but the experience has never been easy.

Recently, I tried again, using Docker approach, and it is amazingly smooth. Done in 5 minutes!

Stop spending a fortune on paid VPN services. You can get one at $5/month on Digital Ocean. For me, I didn't spend a cent, since I am using existing instance that already runs my stuff (:


## The Pre-requisite

First you need a [DigitalOcean](https://m.do.co/c/69baaaf5a07b) account.

I have been using Digital Ocean for a long time, and I recommend anyone who requires a dedicated server to use it. Register an account with my [referral link](https://m.do.co/c/69baaaf5a07b), and get FREE $10 (that's 2 months!) to start with.

Set up a droplet running Ubuntu 14.x, in a geographical location of your liking. I actually have 2 droplets with this VPN setup, 1 in Singapore and 1 in US.

Then [install Docker](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-getting-started).


## Setting Up OpenVPN

We are using the work of [kylemanna/docker-openvpn](https://github.com/kylemanna/docker-openvpn) dockerfile.

To setup the docker container (change `vpn.samwize.com` to your domain/IP):

    OVPN_DATA="ovpn-data"
    docker run -v $OVPN_DATA:/etc/openvpn --rm kylemanna/openvpn ovpn_genconfig -u udp://vpn.samwize.com
    docker run -v $OVPN_DATA:/etc/openvpn --rm -it kylemanna/openvpn ovpn_initpki

You will be prompted to enter your EasyRSA passphrase. These are kept in your data volume `ovpn-data`.

Run the docker container:

    docker run -v $OVPN_DATA:/etc/openvpn -d -p 1194:1194/udp --cap-add=NET_ADMIN kylemanna/openvpn

Next, generate the ovpn file for your computer to connect to the VPN later.

Change `CLIENTNAME` to your computer name (eg. `MacBook-Supreme` for me ^^).

    docker run -v $OVPN_DATA:/etc/openvpn --rm -it kylemanna/openvpn easyrsa build-client-full CLIENTNAME nopass
    docker run -v $OVPN_DATA:/etc/openvpn --rm kylemanna/openvpn ovpn_getclient CLIENTNAME > CLIENTNAME.ovpn


## Setting up your Client

`CLIENTNAME.ovpn` is generated on your server.

You need to **transfer the file to your computer**, eg. via SSH or [Cyberduck](https://cyberduck.io).

Down a VPN software. For Mac, we can download [TunnelBlick](https://tunnelblick.net).

Drag the `CLIENTNAME.ovpn` file to TunnelBlick in the menu, and you should be good to use!
