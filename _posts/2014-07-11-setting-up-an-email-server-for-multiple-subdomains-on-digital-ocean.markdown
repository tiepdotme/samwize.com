---
layout: post
title: "Setting up an email server for multiple subdomains (on Digital Ocean)"
date: 2014-07-11 21:47:11 +0800
comments: true
categories: [VPS, Email]
---

Why would someone setup an email server when there is Gmail?

The reasons are plenty: Gmail (or Google Apps) for Business is [no longer free](http://lifehacker.com/5967154/what-should-i-do-now-that-google-apps-accounts-are-no-longer-free). Outlook support for custom domains is [no longer free](http://www.zdnet.com/microsoft-ends-support-for-custom-domains-in-free-email-service-7000028306/). And why store your most important data [on someone else server](http://arstechnica.com/information-technology/2014/02/how-to-run-your-own-e-mail-server-with-your-own-domain-part-1/)?

<!-- more -->

As such, I set up an email server on the [same $5 Digital Ocean instance](http://samwize.com/2014/05/16/migrating-from-shared-webhost-to-vps-for-wordpress/) I have been using.

I referred to the [guide](https://www.digitalocean.com/community/tutorials/how-to-install-iredmail-on-ubuntu-12-04-x64), with some corrections and enhancements, especially on the iRedMail host names and SSL.


## My Objective

To setup a mail server for my new domain name [wahhh.com](http://wahhh.com).

I have these subdomains and want to create these 3 emails: support@app1.wahhh.com , support@app2.wahhh.com , and also admin@wahhh.com

It is good practise to setup an email server on a seperate **subdomain** such as `mail.wahhh.com`


## Setup DNS Server

I am using [Namecheap](http://www.namecheap.com/?aff=68466) nameserver, and so I configure the records as such:

A Record

    mail > 128.123.213.132

MX Records

    @ > mail.wahhh.com
    app1 > mail.wahhh.com
    app2 > mail.wahhh.com

An example of how the configuration will work: 

An email to admin@wahhh.com will use the first MX Record (@ means nothing), which points to the host name `mail.wahhh.com`, which the A Record points to my actual IP address. At the IP address is where the mail server will be installed.


## Installing iRedMail

[iRedMail](http://www.iredmail.org/index.html) is a bundle of various technologies such as Postfix, Dovecot and Roundcube. A bundle makes the installation much much easier.

Find out the [latest version](http://www.iredmail.org/download.html) (0.8.7 is latest in Jul 2014) and install accordingly:

```
wget https://bitbucket.org/zhb/iredmail/downloads/iRedMail-0.8.7.tar.bz2
tar jxvf iRedMail-0.8.7.tar.bz2 && cd iRedMail-0.8.7
bash iRedMail.sh
```

Follow through the GUI installer.

Restart your droplet.


## TXT Records

There are 2 TXT Records to add.

Firstly, add for DKIM, which is found in `/root/iRedMail-0.8.7/iRedMail.tips`. You might need to concatenate the strings. 

The TXT Record looks like the following (must have the quotes):

```
dkim_domainkey > "v=DKIM1; p=ABCDEFG...XYZ"
```

Add another one for SPF, which includes your IP address:

```
@ > "v=spf1 ip4:128.123.213.132 -all"
app1 > "v=spf1 ip4:128.123.213.132 -all"
app2 > "v=spf1 ip4:128.123.213.132 -all"
```


## Adding Virtual Domains and Users

At this point, you can use the iRedAdmin website to add users to your virtual domain. eg. user1@wahhh.com , user2@wahhh.com , support@app1.wahhh.com , etc

You can also add more virtual domains. For example, if you have a new okloh.com, you can add the virtual domain. In addition, you have to configure the A/MX/TXT records similarly for okloh.com.


## SSL Cert

For email server, you **really should** have a SSL cert.

I use [StartCom](https://www.startssl.com/), as mentioned in [ArsTechnica](http://arstechnica.com/information-technology/2014/03/taking-e-mail-back-part-2-arming-your-server-with-postfix-dovecot/), which provides a FREE class 1 SSL. The steps are pretty lenthy from Ars, but they are not for iRedMail.

So, I followed Ars guide on creating the StartCom SSL key and cert, then Digital Ocean guide to setup Apache. 

The steps:

1. Register at StartCom

2. Enter real identity and wait for approval

3. Validation Wizard > Verify for your domain eg wahhh.com

4. Certificate Wizard > Web Server SSL > 4096 keysize, SHA2 (SHA1 if takes forever) > Continue and wait for a minute..

5. Save the private key as `mail-wahhh.key`

6. Continue > Select your domain > Add "mail.wahhh.com" as the sub domain (only 1 sub domain for this free SSL). Wait for their approval, then retrieve the certificate.

7. SSH into your mail server. `cd /etc/ssl/private`

8. `nano mail-wahhh.key` and paste your private key

9. `nano mail-wahhh.pem` and paste your certificate

10. `wget https://www.startssl.com/certs/sub.class1.server.ca.pem` to download their intermediate CA Cert

11. `cat mail-wahhh.pem sub.class1.server.ca.pem > mail-wahhh-chain.pem` to concat and create a chain cert

12. `openssl rsa -in mail-wahhh.key -out mail-wahhh-decrypted.key` > enter your private key password

13. `chown root:root mail-wahhh*` and `chmod 400 mail-wahhh*` to make sure this file is only accessible by root

14. `nano /etc/apache2/sites-available/default-ssl.conf` > Edit the key and cert path

        SSLCertificateFile /etc/ssl/private/mail-wahhh-chain.pem
        SSLCertificateKeyFile /etc/ssl/private/mail-wahhh-decrypted.key

15. `service apache2 restart`

That's it! You have now secured https://mail.wahhh.com (my fictional URL!).