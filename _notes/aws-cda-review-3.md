---
title: AWS Advanced S3 and Athena
layout: post
tags: [aws, cda, s3, athena]
date: 2020-09-16
---
## S3 Advanced
### S3 MFA-Delete
-	MFA (multi factor authentication) forces user to generate a code on a device (usually a mobile phone or hardware) before doing important operations on S3
-	To use MFA-Delete, enable Versioning on the S3 bucket
-	You will need MFA to : permanently delete an object version,	suspend versioning on the bucket
-	You won’t need MFA for:	enabling versioning, listing deleted versions
-	Only the bucket owner (root account) can enable/disable MFA-Delete, MFA-Delete currently can only be enabled using the CLI
### S3 Default Encryption vs Bucket Policies
-	The old way to enable default encryption was to use a bucket policy and refuse any HTTP command without the proper headers
-	The new way is to use the “default encryption” option in S3
-	Note: Bucket Policies are evaluated before “default encryption”
### S3 Access Logs
-	For audit purpose, you may want to log all access to S3 buckets.	Any request made to S3, from any account, authorized or denied, will be logged into another S3 bucket.
-	That data can be analyzed using data analysis tools or Amazon Athena.
-	The log format is at: https://docs.aws.amazon.com/AmazonS3/latest/dev/LogFormat.html
- S3 Access Logs: Warning:	Do not set your logging bucket to be the monitored bucket.	It will create a logging loop, and your bucket will grow in size exponentially.
### S3 Replication (CRR & SRR)
- Must enable versioning in source and destination
- Cross Region Replication (CRR) - Use cases: compliance, lower latency access, replication across accounts
- Same Region Replication (SRR) - Use cases: log aggregation, live replication between production and test accounts
-	Buckets can be in different accounts. Copying is asynchronous. Must give proper IAM permissions to S3
 


