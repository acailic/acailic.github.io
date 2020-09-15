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
- After activating, only new objects are replicated (not retroactive)
- For DELETE operations:
If you delete without a version ID, it adds a delete marker, not replicated.
If you delete with a version ID, it deletes in the source, not replicated.
-	There is no “chaining” of replication
### S3 Pre-Signed	URLs
- Can generate pre-signed URLs using SDK or CLI. 	For downloads (easy, can use the CLI). For uploads (harder, must use the SDK)
- Valid for a default of 3600 seconds, can change timeout with --expires-in [TIME_BY_SECONDS] argument
- Users given a pre-signed URL inherit the permissions of the person who generated the URL for GET / PUT
- Use case: only logged in users for premium video on s3. Allow an list of users to download files by URL dynamicly. Allow temporary a user to upload a file to a precise location. 
- aws configure set default.s3.signature_version s3v4
- aws s3 presign s3:/link --expires-in 100 --region eu-west-1 

## S3 Storage Classes
- https://aws.amazon.com/s3/storage-classes/
### Amazon S3 Standard - General Purpose
- High durability (99.999999999%) of objects across multiple AZ, 99.99% Availability over a given year
- If you store 10,000,000 objects with Amazon S3, you can on average expect to incur a loss of a single object once every 10,000 years
-	Sustain 2 concurrent facility failures
- Use Cases: Big Data analytics, mobile & gaming applications, content distribution…

### Amazon S3 Standard-Infrequent Access (IA)

- Suitable for data that is less frequently accessed, but requires rapid access when needed
- High durability (99.999999999%) of objects across multiple AZs, 99.9% Availability
- Low cost compared to Amazon S3 Standard
- Sustain 2 concurrent facility failures
- Use Cases: As a data store for disaster recovery, backups…

### Amazon S3 One Zone-Infrequent Access
- Same as IA but data is stored in a single AZ
- High durability (99.999999999%) of objects in a single AZ; data lost when AZ is destroyed, 99.5% Availability
- Low latency and high throughput performance
- Supports SSL for data at transit and encryption at rest
- Low cost compared to IA (by 20%)
- Use Cases: Storing secondary backup copies of on-premise data, or storing data you can recreate

### Amazon S3 Intelligent Tiering
- Same low latency and high throughput performance of S3 Standard
- Small monthly monitoring and auto-tiering fee
- Automatically moves objects between two access tiers based on changing access patterns
- Designed for durability of 99.999999999% of objects across multiple Availability Zones
- Resilient against events that impact an entire Availability Zone
- Designed for 99.9% availability over a given year

### Amazon Glacier
- Low cost object storage meant for archiving / backup
- Data is retained for the longer term (10s of years)
- Alternative to on-premise magnetic tape storage
- Average annual durability is 99.999999999%
- Cost per storage per month ($0.004 / GB) + retrieval cost
- Each item in Glacier is called “Archive” (up to 40TB),	Archives are stored in ”Vaults”
-	Amazon Glacier – 3 retrieval options:	Expedited (1 to 5 minutes),Standard (3 to 5 hours),Bulk (5 to 12 hours),
-	Minimum storage duration of 90 days

### Amazon Glacier Deep Archive
-	Amazon Glacier Deep Archive – for long term storage – cheaper:Standard (12 hours),	Bulk (48 hours),
-	Minimum storage duration of 180 days

### S3 Lifecycle policies
- You can transition objects between storage classes. Moving objects can be automated using a lifecycle configuration
-	For infrequently accessed object, move them to STANDARD_IA
-	For archive objects you don’t need in real-time, GLACIER or DEEP_ARCHIVE
- Transition actions: It defines when objects are transitioned to another storage class.Move objects to Standard IA class 60 days after creation.	Move to Glacier for archiving after 6 months
-	Expiration actions: configure objects to expire (delete) after some time
- Access log files can be set to delete after a 365 days
-	Can be used to delete old versions of files (if versioning is enabled).	Can be used to delete incomplete multi-part uploads
-	Rules can be created for a certain prefix (ex - s3://mybucket/mp3/*).	Rules can be created for certain objects tags (ex - Department: Finance)
- exp: S3 source images can be on STANDARD, with a lifecycle configuration to transition them to GLACIER after 45 days. S3 thumbnails can be on ONEZONE_IA, with a lifecycle configuration to expire them (delete them) after 45 days.
- exp: 	You need to enable S3 versioning in order to have object versions, so that “deleted objects” are in fact hidden by a “delete marker” and can be recovered.	You can transition these “noncurrent versions” of the object to S3_IA. You can transition afterwards these “noncurrent versions” to DEEP_ARCHIVE

### S3 baseline performance
-	Amazon S3 automatically scales to high request rates, latency 100-200 ms
-	Your application can achieve at least 3,500 PUT/COPY/POST/DELETE and 5,500 GET/HEAD requests per second per prefix in a bucket.
-	There are no limits to the number of prefixes in a bucket.
•	Example (object path => prefix):
•	bucket/folder1/sub1/file	=> /folder1/sub1/
•	bucket/folder1/sub2/file	=> /folder1/sub2/
•	bucket/1/file	=> /1/
•	bucket/2/file	=> /2/
-	If you spread reads across all four prefixes evenly, you can achieve 22,000 requests per second for GET and HEAD
#### S3 KMS limitation
- if you use SSE-KMS, you may be impacted by the KMS limits. Count towards the KMS quota per second (5500, 10000, 30000 req/s based on region)
-	When you upload, it calls the GenerateDataKey KMS API.	When you download, it calls the Decrypt KMS API 
-	As of today, you cannot request a quota increase for KMS
#### S3 Performance
-	Multi-Part upload:	recommended for files > 100MB, must use for files > 5GB.	Can help parallelize uploads (speed up transfers)
- S3 Transfer Acceleration (upload only):	Increase transfer speed by transferring file to an AWS edge location which will forward the data to the S3 bucket in the target region. Compatible with multi-part upload.
- S3 Byte-Range Fetches-Parallelize GETs by requesting specific byte ranges.	Better resilience in case of failures

### S3 and Glacier Select
- Retrieve less data using SQL by performing server side filtering
- Can filter by rows & columns (simple SQL statements); like filtering CSV files
- Less network transfer, less CPU cost client-side
- https://aws.amazon.com/blogs/aws/s3-glacier-select/

### S3 Lock Policies and Glacier Lock
- S3 Object Lock:	Adopt a WORM (Write Once Read Many) model
-	Block an object version deletion for a specified amount of time
- Glacier Vault Lock: Adopt a WORM (Write Once Read Many) model
- Lock the policy for future edits (can no longer be changed)
- Helpful for compliance and data retention


### S3 Event Notification
- S3:ObjectCreated, S3:ObjectRemoved, S3:ObjectRestore, S3:Replication…
- Object name filtering possible (*.jpg)
- Use case: generate thumbnails of images uploaded to S3
- Can create as many “S3 events” as desired
- S3 event notifications typically deliver events in seconds but can sometimes take a minute or longer
- If two writes are made to a single non-versioned object at the same time, it is possible that only a single event notification will be sent
- If you want to ensure that an event notification is sent for every successful write, you can enable versioning on your bucket.

## AWS Athena
- Serverless service to perform analytics directly against S3 files
- Uses SQL language to query the files
- Has a JDBC / ODBC driver
- Charged per query and amount of data scanned
- Supports CSV, JSON, ORC, Avro, and Parquet (built on Presto)
- Use cases: Business intelligence / analytics / reporting, analyze & query VPC Flow Logs, ELB Logs, CloudTrail trails, etc...
- Exam Tip: Analyze data directly on S3 => use Athena


### questions
- MFA Delete forces users to use MFA tokens before deleting objects. It's an extra level of security to prevent accidental deletes
- S3 CRR is used to replicate data from an S3 bucket to another one in a different region
- Pre-Signed URL are temporary and grant time-limited access to some actions in your S3 bucket.
- When a file is over 100 MB, Multi Part upload is recommended as it will upload many parts in parallel, maximizing the throughput of your bandwidth and also allowing for a smaller part to retry in case that part fails.
- Athena -Serverless data analysis service allowing you to query data in S3
 


