---
layout: post
title: "Set Up Your Own VPN Server With $5"
date: 2018-06-04T00:46:32+08:00
categories: []
---

This is a very cheap way to run your dedicated VPN server, with $5/month on [DigitalOcean](https://m.do.co/c/69baaaf5a07b).

## 1. Sign Up DigitalOcean

I have been using Digital Ocean for a long time and highly recommend it when you need a dedicated server.

Sign up DigitalOcean with my [referral link](https://m.do.co/c/69baaaf5a07b), and get **FREE $10 (that's 2 months!)** to start with.

When you do not need the VPN server any longer, you may shut it down anytime, and you won't incur a thing.

## 2. Setup for SSH

Before you create your droplet, you must setup your computer for ssh-ing later on.

If you have never setup your RSA key pair, then you will have to run `ssh-keygen` first.

Copy the public key,

    cat ~/.ssh/id_rsa.pub|pbcopy

Then paste that in DigitalOcean > Settings > Security > Add SSH Key.

## 3. Create Droplet (the server)

Create Droplets > One-click apps > Docker 17.12 on 16.04

Choose the cheapest $5/month droplet size, with 1 GB memory etc.

Choose the datacenter region which you want to VPN to eg. Singapore

Create a hostname eg. sg-vpn

Note down the IP for your droplet (the ipv4). My example uses 128.199.123.123.

## 4. Install OpenVPN

Now we get into the server and install the VPN service.

    ssh root@128.199.123.123

If you have trouble SSH in, make sure you setup your SSH keys in step 2 (will need to recreate droplet).

Run these docker commands, enter a PEM pass phrase when prompted (mandatory). Give a Common Name eg. "sg-vpn".

    OVPN_DATA="ovpn-data"
    docker run -v $OVPN_DATA:/etc/openvpn --rm kylemanna/openvpn ovpn_genconfig -u udp://128.199.123.123
    docker run -v $OVPN_DATA:/etc/openvpn --rm -it kylemanna/openvpn ovpn_initpki
    docker run -v $OVPN_DATA:/etc/openvpn -d -p 1194:1194/udp --cap-add=NET_ADMIN kylemanna/openvpn

Next, generate the ovpn file -- a configuration that your computer can import later. In the example I have "sg-vpn.ovpn".

    docker run -v $OVPN_DATA:/etc/openvpn --rm -it kylemanna/openvpn easyrsa build-client-full CLIENTNAME nopass
    docker run -v $OVPN_DATA:/etc/openvpn --rm kylemanna/openvpn ovpn_getclient CLIENTNAME > sg-vpn.ovpn

We need to now exit ssh remote and go back to your computer.

    exit

## 5. Install TunnelBlick

On your computer, download the ovpn file using scp.

    # This copy the ovpn file (in /root) to the current directory you're in
    scp -r root@128.199.123.123:/root/sg-vpn.ovpn .

Install [TunnelBlick](https://tunnelblick.net), a VPN client. You may use others.

Open or import the ovpn file, and enjoy your VPN (:
