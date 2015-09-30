---
layout: post
title: "s3cmd broken pipe error (errno 32)"
date: 2013-04-21 13:26
comments: true
categories: AWS
---

If you use [s3cmd](http://s3tools.org/s3cmd) and encountered `[Errno 32] Broken pipe` as you try to put an object in a bucket, understand that this is a _very very bad error message_.

[Jeremy](http://jeremyshapiro.com/blog/2011/02/errno-32-broken-pipe-in-s3cmd/) blogged about this and attributed the error to **a typo in the bucket name**.

Others attributed it to **no permission**, **file too big**, etc..

I attributed it to **incorrect permission policy**.

<!-- more -->

I was trying to create a new security group, and adding a new user, and restrict his access to only 1 of my S3 bucket. When you create a new security group, you can [edit the policy](http://docs.aws.amazon.com/IAM/latest/UserGuide/PoliciesOverview.html).

You might change to this, thinking it allows all action on `example_bucket`:

```json WRONG Policy
{
  "Statement":[{
     "Effect":"Allow",
     "Action":"*",
     "Resource":"arn:aws:s3:::example_bucket"
   }]
}
```

You are wrong (though I say Amazon and it's documentation to blame).

The [correct way](http://stackoverflow.com/a/6955864/242682) is to have multiple statements like this:

```json Correct Policy
{
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:ListBucket",
        "s3:GetBucketLocation",
        "s3:ListBucketMultipartUploads"
      ],
      "Resource": "arn:aws:s3:::example_bucket",
      "Condition": {}
    },
    {
      "Effect": "Allow",
      "Action": [
        "s3:AbortMultipartUpload",
        "s3:DeleteObject",
        "s3:DeleteObjectVersion",
        "s3:GetObject",
        "s3:GetObjectAcl",
        "s3:GetObjectVersion",
        "s3:GetObjectVersionAcl",
        "s3:PutObject",
        "s3:PutObjectAcl",
        "s3:PutObjectAclVersion"
      ],
      "Resource": "arn:aws:s3:::example_bucket/*",
      "Condition": {}
    }
  ]
}
```

You need to split into 2 resources.

1. `arn:aws:s3:::example_bucket` - allow to list objects in the bucket

2. `arn:aws:s3:::example_bucket/*` - allow to Get/Put/etc the objects in the bucket

It's subtle..

