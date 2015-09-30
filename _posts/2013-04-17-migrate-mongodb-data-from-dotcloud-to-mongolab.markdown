---
layout: post
title: "Migrate mongodb data from Dotcloud to MongoLab"
date: 2013-04-17 02:50
comments: true
categories: Dotcloud
---

Similar to how you [transfer data away from Dotcloud](/2013/04/01/transfer-away-dotcloud-data-using-ftp/), you can do the same for your mongodb database.

I have been moving away from Dotcloud as they no longer provide free sandbox app.

<!-- more -->

```
// ssh into mongo db instance first
dot cloud run db

// dump the data, then transfer via ftp
mongodump -h mongo.MYAPP.dotcloud.com:MYPORT -u root -p MYPASSWORD -d MYDBNAME
tar -czf MYDBNAME.tgz dump
curl -u myftpusername:myftppassword -sST MYDBNAME.tgz ftp://myftpdomain.com
```

I use FTP, but if you prefer S3, you could do [this](http://icodesnip.com/snippet/bash/dotcloud-mongodb-backup-script).

Download the tgz and unzip to get a dump directory.

Create a database in mongolab.


```
// On local machine, do a mongorestore to mongolab
mongorestore -h xxx037077.mongolab.com:37077 -d MYDBNAME -u root -p MYPASSWORD2 dump/MYAPP/
```

