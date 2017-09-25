---
layout: post
title: AWS Policy - Full S3 Access for a Single Bucket
---

I've found myself googling this every couple months. So I'm posting an example
here to save you and myself in the future.

If you're creating an API user with full access to only a SINGLE S3 bucket, this
example policy will work for you.

If you want to reduce the number of actions they can do, you'll have to list
them in `Action`.

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "Stmt1506199387000",
            "Effect": "Allow",
            "Action": [
                "s3:*"
            ],
            "Resource": [
                "arn:aws:s3:::your-bucket-name/*"
            ]
        }
    ]
}
```
