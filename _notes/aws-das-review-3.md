---
title: AWS DAS Storage 
layout: post
tags: [aws, das, s3, dynamodb]
date: 2020-10-01
---
## AWS S3 Overview - Buckets
Amazon S3 allows people to store objects (files) in “buckets”
(directories)
Buckets must have a globally unique name
Buckets are defined at the region level
Naming convention
No uppercase
No underscore
3-63 characters long
Not an IP
Must start with lowercase letter or number
### AWS S3 Overview - Objects
Objects (files) have a Key. The key is the FULL path:
<my_bucket>/my_file.txt
<my_bucket>/my_folder1/another_folder/my_file.txt
There’s no concept of “directories” within buckets
(although the UI will trick you to think otherwise)
Just keys with very long names that contain slashes (“/”)
Object Values are the content of the body:
Max Size is 5TB
If uploading more than 5GB, must use “multi-part upload”
Metadata (list of text key / value pairs – system or user metadata)
Tags (Unicode key / value pair – up to 10) – useful for security / lifecycle
Version ID (if versioning is enabled)
### AWS S3 - Consistency Model
Read after write consistency for PUTS of new objects
As soon as an object is written, we can retrieve it ex: (PUT 200 -> GET 200)
This is true, except if we did a GET before to see if the object existed ex: (GET 404 -> PUT 200 -> GET 404) – eventually consistent
Eventual Consistency for DELETES and PUTS of existing objects
If we read an object after updating, we might get the older version ex: (PUT 200 -> PUT 200 -> GET 200 (might be older version))
If we delete an object, we might still be able to retrieve it for a short time ex: (DELETE 200 -> GET 200)
### S3 Storage Classes
Amazon S3 Standard - General Purpose
Amazon S3 Standard-Infrequent Access (IA)
Amazon S3 One Zone-Infrequent Access
Amazon S3 Intelligent Tiering
Amazon Glacier
Amazon Glacier Deep Archive
Amazon S3 Reduced Redundancy Storage (deprecated - omitted)
### S3 Standard – General Purpose
High durability (99.999999999%) of objects across multiple AZ
If you store 10,000,000 objects with Amazon S3, you can on average expect to incur a loss of a single object once every 10,000 years
99.99% Availability over a given year
Sustain 2 concurrent facility failures
Use Cases: Big Data analytics, mobile & gaming applications, content distribution…
### S3 Standard – Infrequent Access (IA)
Suitable for data that is less frequently accessed, but requires rapid access when needed
High durability (99.999999999%) of objects across multiple AZs
99.9% Availability
Low cost compared to Amazon S3 Standard
Sustain 2 concurrent facility failures
Use Cases: As a data store for disaster recovery, backups…
### S3 One Zone - Infrequent Access (IA)
Same as IA but data is stored in a single AZ
High durability (99.999999999%) of objects in a single AZ; data lost when AZ is destroyed
99.5% Availability
Low latency and high throughput performance
Supports SSL for data at transit and encryption at rest
Low cost compared to IA (by 20%)
Use Cases: Storing secondary backup copies of on-premise data, or storing data you can recreate
### S3 Intelligent Tiering
Same low latency and high throughput performance of S3 Standard
Small monthly monitoring and auto-tiering fee
Automatically moves objects between two access tiers based on changing access patterns
Designed for durability of 99.999999999% of objects across multiple Availability Zones
Resilient against events that impact an entire Availability Zone
Designed for 99.9% availability over a given year
### Amazon Glacier
Low cost object storage meant for archiving / backup
Data is retained for the longer term (10s of years)
Alternative to on-premise magnetic tape storage
Average annual durability is 99.999999999%
Cost per storage per month ($0.004 / GB) + retrieval cost
Each item in Glacier is called “Archive” (up to 40TB)
Archives are stored in ”Vaults”
### Amazon Glacier & Glacier Deep Archive
Amazon Glacier – 3 retrieval options:
Expedited (1 to 5 minutes)
Standard (3 to 5 hours)
Bulk (5 to 12 hours)
Minimum storage duration of 90 days
Amazon Glacier Deep Archive – for long term storage – cheaper:
Standard (12 hours)
Bulk (48 hours)
Minimum storage duration of 180 days
### S3 Storage Classes Comparison
### S3 – Moving between storage classes
You can transition objects between storage classes
For infrequently accessed object, move them to
STANDARD_IA
For archive objects you don’t need in real-time, GLACIER or
DEEP_ARCHIVE
Moving objects can be automated using a lifecycle configuration
### S3 Lifecycle Rules
Transition actions: It defines when objects are transitioned to another storage class.
Move objects to Standard IA class 60 days after creation
Move to Glacier for archiving after 6 months
Expiration actions: configure objects to expire (delete) after some time
Access log files can be set to delete after a 365 days
Can be used to delete old versions of files (if versioning is enabled)
Can be used to delete incomplete multi-part uploads
Rules can be created for a certain prefix (ex - s3://mybucket/mp3/*)
Rules can be created for certain objects tags (ex - Department: Finance)
### AWS S3 - Versioning
You can version your files in AWS S3
It is enabled at the bucket level
Same key overwrite will increment the “version”: 1, 2, 3….
It is best practice to version your buckets
Protect against unintended deletes (ability to restore a version)
Easy roll back to previous version
Any file that is not versioned prior to enabling versioning will have version “null”
You can “suspend” versioning
### S3 Cross Region Replication
Must enable versioning (source and destination)
Buckets must be in different AWS regions
• Can be in different accounts
Asynchronous
• Copying is asynchronous
replication
• Must give proper IAM permissions
to S3
eu-west-1
us-east-1
Use cases: compliance, lower latency access, replication across accounts
### AWS S3 – ETag (Entity Tag)
How do you verify if a file has already been uploaded to S3?
Names work, but how are you sure the file is exactly the same?
For this, you can use AWS ETags:
For simple uploads (less than 5GB), it’s the MD5 hash
For multi-part uploads, it’s more complicated, no need to know the algorithm
Using ETag, we can ensure integrity of files
### AWS S3 Performance – Key Names Historic fact and current exam
When you had > 100 TPS (transaction per second), S3 performance could degrade
Behind the scene, each object goes to an S3 partition and for the best performance, we want the highest partition distribution
In the exam, and historically, it was recommended to have random characters in front of your key name to optimise performance:
<my_bucket>/5r4d_my_folder/my_file1.txt
<my_bucket>/a91e_my_folder/my_file2.txt
…
It was recommended never to use dates to prefix keys:
<my_bucket>/2018_09_09_my_folder/my_file1.txt
<my_bucket>/2018_09_10_my_folder/my_file2.txt
### AWS S3 Performance – Key Names Current performance (not yet exam)
https://aws.amazon.com/about-aws/whats-new/2018/07/amazon-s3-announces-increased-request-rate-performance/
As of July 17th 2018, we can scale up to 3500 RPS for PUT and 5500 RPS for GET for EACH PREFIX
“This S3 request rate performance increase removes any previous guidance to randomize object prefixes to achieve faster performance”
It’s a “good to know”, until the exam gets updated ☺
### AWS S3 Performance
Faster upload of large objects (>5GB), use multipart upload:
parallelizes PUTs for greater throughput
maximize your network bandwidth
decrease time to retry in case a part fails
Use CloudFront to cache S3 objects around the world (improves reads)
S3 Transfer Acceleration (uses edge locations) – just need to change the endpoint you write to, not the code.
If using SSE-KMS encryption, you may be limited to your AWS limits for KMS usage (~100s – 1000s downloads / uploads per second)
### S3 Encryption for Objects
There are 4 methods of encrypting objects in S3
SSE-S3: encrypts S3 objects using keys handled & managed by AWS
SSE-KMS: leverage AWS Key Management Service to manage encryption keys
SSE-C: when you want to manage your own encryption keys
Client Side Encryption
It’s important to understand which ones are adapted to which situation for the exam
### S3 Encryption for Objects: SSE-S3
SSE-S3: encryption using keys handled & managed by AWS S3
Object is encrypted server side
AES-256 encryption type
Must set header:   “x-amz-server-side-encryption": "AES256"
### S3 Encryption for Objects: SSE-KMS
SSE-KMS: encryption using keys handled & managed by KMS
KMS Advantages: user control + audit trail
Object is encrypted server side
Must set header:   “x-amz-server-side-encryption": ”aws:kms"
### S3 Encryption for Objects: SSE-C
SSE-C: server-side encryption using data keys fully managed by the customer outside of AWS
Amazon S3 does not store the encryption key you provide
HTTPS must be used
Encryption key must provided in HTTP headers, for every HTTP request made
### S3 Encryption for Objects: Client Side Encryption
Client library such as the Amazon S3 Encryption Client
Clients must encrypt data themselves before sending to S3
Clients must decrypt data themselves when retrieving from S3
Customer fully manages the keys and encryption cycle
### S3 Encryption for Objects: Encryption in transit (SSL)
AWS S3 exposes:
HTTP endpoint: non encrypted
HTTPS endpoint: encryption in flight
You’re free to use the endpoint you want, but HTTPS is recommended
HTTPS is mandatory for SSE-C
Encryption in flight is also called SSL / TLS
### S3 CORS (Cross-Origin Resource Sharing)
If you request data from another website, you need to enable CORS
Cross Origin Resource Sharing allows you to limit the number of websites that can request your files in S3 (and limit your costs)
It’s a popular exam question
### S3 Access Logs
For audit purpose, you may want to log all access to S3 buckets
Any request made to S3, from any account, authorized or denied, will be logged into another S3 bucket
That data can be analyzed using data analysis tools…
Or Amazon Athena as we’ll see later in this course!
The log format is at: https://docs.aws.amazon.com/AmazonS3/latest/dev/Lo gFormat.html
### S3 Security
### S3 Bucket Policies
### S3 Security - Other