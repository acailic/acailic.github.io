---
title: AWS DAS Storage 
layout: post
tags: [aws, das, s3, dynamodb]
date: 2020-10-01
---
## AWS S3 Overview - Buckets
Amazon S3 allows people to store objects (files) in “buckets”(directories)
Buckets must have a globally unique name
Buckets are defined at the region level
Naming convention
No uppercase
No underscore
3-63 characters long
Not an IP
Must start with lowercase letter or number
### AWS S3 Overview - Objects
- Objects (files) have a Key. The key is the FULL path:<my_bucket>/my_file.txt,<my_bucket>/my_folder1/another_folder/my_file.txt
- There’s no concept of “directories” within buckets (although the UI will trick you to think otherwise)
- Just keys with very long names that contain slashes (“/”)
- Object Values are the content of the body:
- Max Size is 5TB
- If uploading more than 5GB, must use “multi-part upload”
- Metadata (list of text key / value pairs – system or user metadata)
- Tags (Unicode key / value pair – up to 10) – useful for security / lifecycle
- Version ID (if versioning is enabled)
### AWS S3 - Consistency Model
Read after write consistency for PUTS of new objects
As soon as an object is written, we can retrieve it ex: (PUT 200 -> GET 200)
This is true, except if we did a GET before to see if the object existed ex: (GET 404 -> PUT 200 -> GET 404) – eventually consistent
Eventual Consistency for DELETES and PUTS of existing objects
If we read an object after updating, we might get the older version ex: (PUT 200 -> PUT 200 -> GET 200 (might be older version))
If we delete an object, we might still be able to retrieve it for a short time ex: (DELETE 200 -> GET 200)
### S3 Storage Classes
- Amazon S3 Standard - General Purpose
- Amazon S3 Standard-Infrequent Access (IA)
- Amazon S3 One Zone-Infrequent Access
- Amazon S3 Intelligent Tiering
- Amazon Glacier
- Amazon Glacier Deep Archive
- Amazon S3 Reduced Redundancy Storage (deprecated - omitted)
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
In the exam, and historically, it was recommended to have random characters in front of your key name to optimise performance:<my_bucket>/5r4d_my_folder/my_file1.txt,<my_bucket>/a91e_my_folder/my_file2.txt
It was recommended never to use dates to prefix keys:<my_bucket>/2018_09_09_my_folder/my_file1.txt,<my_bucket>/2018_09_10_my_folder/my_file2.txt
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
The log format is at: https://docs.aws.amazon.com/AmazonS3/latest/dev/LogFormat.html
### S3 Security
User based
IAM policies - which API calls should be allowed for a specific user from IAM console
Resource Based
Bucket Policies - bucket wide rules from the S3 console - allows cross account
Object Access Control List (ACL) – finer grain
Bucket Access Control List (ACL) – less common
### S3 Bucket Policies
JSON based policies
Resources: buckets and objects
Actions: Set of API to Allow or Deny
Effect: Allow / Deny
Principal: The account or user to apply the policy to
Use S3 bucket for policy to:
Grant public access to the bucket
Force objects to be encrypted at upload
Grant access to another account (Cross Account)
### S3 Default Encryption vs Bucket Policies
The old way to enable default encryption was to use a bucket policy and refuse any HTTP command without the proper headers:
The new way is to use the “default encryption” option in S3
Note: Bucket Policies are evaluated before “default encryption”
### S3 Security - Other
Networking:
Supports VPC Endpoints (for instances in VPC without www internet)
Logging and Audit:
S3 access logs can be stored in other S3 bucket
API calls can be logged in AWS CloudTrail
User Security:
MFA (multi factor authentication) can be required in versioned buckets to delete objects
Signed URLs: URLs that are valid only for a limited time (ex: premium video service for logged in users)
### Glacier
Low cost object storage meant for archiving / backup
Data is retained for the longer term (10s of years)
Alternative to on-premise magnetic tape storage
Average annual durability is 99.999999999%
Cost per storage per month ($0.004 / GB) + retrieval cost
Each item in Glacier is called “Archive” (up to 40TB)
Archives are stored in ”Vaults”
Exam tip: archival from S3 after XXX days => use Glacier
###  Glacier Operations
Restore links have an expiry date
3 retrieval options:
Expedited (1 to 5 minutes retrieval) – $0.03 per GB and $0.01 per request
Standard (3 to 5 hours) - $0.01 per GB and 0.05 per 1000 requests
Bulk (5 to 12 hours) - $0.0025 per GB and $0.025 per 1000 requests
### Glacier - Vault Policies & Vault Lock
Vault is a collection of archives
Each Vault has:
ONE vault access policy
ONE vault lock policy
Vault Policies are written in JSON
Vault Access Policy is similar to bucket policy (restrict user / account permissions)
Vault Lock Policy is a policy you lock, for regulatory and compliance requirements.
The policy is immutable, it can never be changed (that’s why it’s call LOCK)
Example 1: forbid deleting an archive if less than 1 year old
Example 2: implement WORM policy (write once read many)
### S3 Select & Glacier Select
Retrieve less data using SQL by performing server side filtering
Can filter by rows & columns (simple SQL statements)
Less network transfer, less CPU cost client-side
### S3 Select with Hadoop
Transfer some data from S3 before analyzing it with your cluster
Load less data into Hadoop, save network costs, transfer the data faster
CSV file
Send filtered dataset
Amazon S3 Using S3 Select
Server-side filtering
### DynamoDB
Fully Managed, Highly available with replication across 3 AZ
NoSQL database - not a relational database
Scales to massive workloads, distributed database
Millions of requests per seconds, trillions of row, 100s of TB of storage
Fast and consistent in performance (low latency on retrieval)
Integrated with IAM for security, authorization and administration
Enables event driven programming with DynamoDB Streams
Low cost and auto scaling capabilities
### DynamoDB - Basics
DynamoDB is made of tables
Each table has a primary key (must be decided at creation time)
Each table can have an infinite number of items (= rows)
Each item has attributes (can be added over time – can be null)
Maximum size of a item is 400KB
Data types supported are:
Scalar Types: String, Number, Binary, Boolean, Null
Document Types: List, Map
Set Types: String Set, Number Set, Binary Set
### DynamoDB – Primary Keys
- Option 1: Partition key only (HASH)
Partition key must be unique for each item
Partition key must be “diverse” so that the data is distributed
Example: user_id for a users table
- Option 2: Partition key + Sort Key
The combination must be unique
Data is grouped by partition key
Sort key == range key
Example: users-games table
user_id for the partition key
game_id for the sort key
### DynamoDB – Partition Keys exercise
- We’re building a movie database
- What is the best partition key to maximize data distribution?
•  movie_id
• producer_name
• leader_actor_name
• movie_language
• movie_id has the highest cardinality so it’s a good candidate
• moving_language doesn’t take many values and may be skewed towards English so it’s not a great partition key
### DynamoDB in Big Data
- Common use cases include:
• Mobile apps
• Gaming
• Digital ad serving
• Live voting
• Audience interaction for live events
• Sensor networks
• Log ingestion
• Access control for web-based content
• Metadata storage for Amazon S3 objects
• E-commerce shopping carts
• Web session management
### Anti Pattern
- Prewritten application tied to a traditional relational database: use RDS instead
- Joins or complex transactions
- Binary Large Object (BLOB) data: store data in S3 & metadata in DynamoDB
- Large data with low I/O rate: use S3 instead
### DynamoDB – Provisioned Throughput
- Table must have provisioned read and write capacity units
- Read Capacity Units (RCU): throughput for reads
- Write Capacity Units (WCU): throughput for writes
- Option to setup auto-scaling of throughput to meet demand
- Throughput can be exceeded temporarily using “burst credit”
- If burst credit are empty, you’ll get a “ProvisionedThroughputException”.
- It’s then advised to do an exponential back-off retry
### DynamoDB – Write Capacity Units
- One write capacity unit represents one write per second for an item up to 1 KB in size.
- If the items are larger than 1 KB, more WCU are consumed
- Example 1: we write 10 objects per seconds of 2 KB each.
- We need 2 * 10 = 20 WCU
- Example 2: we write 6 objects per second of 4.5 KB each
- We need 6 * 5 = 30 WCU  (4.5 gets rounded to the upper KB)
- Example 3: we write 120 objects per minute of 2 KB each
- We need 120 / 60 * 2 = 4 WCU
### Strongly Consistent Read vs Eventually Consistent Read
- Eventually Consistent Read: If we read just after a write, it’s possible we’ll get unexpected response because of replication
- Strongly Consistent Read: If we read just after a write, we will get the correct data
- By default: DynamoDB uses Eventually Consistent Reads, but GetItem, Query & Scan provide a
“ConsistentRead” parameter you can set to True
### DynamoDB – Read Capacity Units
- One read capacity unit represents one strongly consistent read per second, or two eventually consistent reads per second, for an item up to 4 KB in size.
- If the items are larger than 4 KB, more RCU are consumed
- Example 1: 10 strongly consistent reads per seconds of 4 KB each
- We need 10 * 4 KB / 4 KB = 10 RCU
- Example 2: 16 eventually consistent reads per seconds of 12 KB each
- We need (16 / 2) * ( 12 / 4 ) = 24 RCU
- Example 3: 10 strongly consistent reads per seconds of 6 KB each
- We need 10 * 8 KB / 4 = 20 RCU (we have to round up 6 KB to 8 KB)
### DynamoDB - Throttling
- If we exceed our RCU or WCU, we get
- ProvisionedThroughputExceededExceptions
- Reasons:
• Hot keys / partitions: one partition key is being read too many times (popular item for ex)
• Very large items: remember RCU and WCU depends on size of items
- Solutions:
• Exponential back-off when exception is encountered (already in SDK)
• Distribute partition keys as much as possible
• If RCU issue, we can use DynamoDB Accelerator (DAX)
### DynamoDB – Partitions Internal
### DynamoDB – Writing Data
- PutItem - Write data to DynamoDB (create data or full replace)
• Consumes WCU
- UpdateItem – Update data in DynamoDB (partial update of attributes)
• Possibility to use Atomic Counters and increase them
- Conditional Writes:
• Accept a write / update only if conditions are respected, otherwise reject
• Helps with concurrent access to items
- No performance impact
### DynamoDB – Deleting Data
- DeleteItem
• Delete an individual row
• Ability to perform a conditional delete
- DeleteTable
• Delete a whole table and all its items
- Much quicker deletion than calling DeleteItem on all items
### DynamoDB – Batching Writes
- BatchWriteItem
- Up to 25 PutItem and / or DeleteItem in one call
- Up to 16 MB of data written
- Up to 400 KB of data per item
- Batching allows you to save in latency by reducing the number of API calls done against DynamoDB
- Operations are done in parallel for better efficiency
- It’s possible for part of a batch to fail, in which case we have the try the failed items (using exponential back-off algorithm)
### DynamoDB – Reading Data
- GetItem:
- Read based on Primary key
- Primary Key = HASH or HASH-RANGE
- Eventually consistent read by default
- Option to use strongly consistent reads (more RCU - might take longer)
- ProjectionExpression can be specified to include only certain attributes
- BatchGetItem:
Up to 100 items
Up to 16 MB of data
Items are retrieved in parallel to minimize latency
### DynamoDB – Query
- Query returns items based on:
- PartitionKey value (must be = operator)
- SortKey value (=, <, <=, >, >=, Between, Begin) – optional
- FilterExpression to further filter (client side filtering)
- Returns:
- Up to 1 MB of data
- Or number of items specified in Limit
- Able to do pagination on the results
- Can query table, a local secondary index, or a global secondary index
### DynamoDB - Scan
- Scan the entire table and then filter out data (inefficient)
- Returns up to 1 MB of data – use pagination to keep on reading
- Consumes a lot of RCU
- Limit impact using Limit or reduce the size of the result and pause
- For faster performance, use parallel scans:
- Multiple instances scan multiple partitions at the same time
- Increases the throughput and RCU consumed
- Limit the impact of parallel scans just like you would for Scans
- Can use a ProjectionExpression + FilterExpression (no change to RCU)
### DynamoDB – LSI (Local Secondary Index)
- Alternate range key for your table, local to the hash key
- Up to five local secondary indexes per table.
- The sort key consists of exactly one scalar attribute.
- The attribute that you choose must be a scalar String, Number, or Binary
- LSI must be defined at table creation time
### DynamoDB – GSI (Global Secondary Index)
- To speed up queries on non-key attributes, use a Global Secondary Index
- GSI = partition key + optional sort key
- The index is a new “table” and we can project attributes on it
- The partition key and sort key of the original table are always projected (KEYS_ONLY)
- Can specify extra attributes to project (INCLUDE)
- Can use all attributes from main table (ALL)
- Must define RCU / WCU for the index
- Possibility to add / modify GSI (not LSI)
### DynamoDB - DAX
- DAX = DynamoDB Accelerator
- Seamless cache for DynamoDB, no application re-write
- Writes go through DAX to DynamoDB
- Micro second latency for cached reads & queries
- Solves the Hot Key problem (too many reads)
- 5 minutes TTL for cache by default
- Up to 10 nodes in the cluster
- Multi AZ (3 nodes minimum recommended for production)
- Secure (Encryption at rest with KMS, VPC, IAM, CloudTrail…)
### DynamoDB Streams 
- Changes in DynamoDB (Create, Update, Delete) can end up in a DynamoDB Stream
- This stream can be read by AWS Lambda, and we can then do:
- React to changes in real time (welcome email to new users)
- Create derivative tables / views
- Insert into ElasticSearch
- Could implement Cross Region Replication using Streams
- Stream has 24 hours of data retention
- Configurable batch size (up to 1,000 rows, 6 MB)
### DynamoDB Streams Kinesis Adapter
- Use the KCL library to directly consume from DynamoDB Streams
- You just need to add a “Kinesis Adapter” library
- The interface and programming is exactly the same as Kinesis Streams
- That’s the alternative to using AWS Lambda
### DynamoDB TTL (Time to Live)
- TTL = automatically delete an item after an expiry date / time
- TTL is provided at no extra cost, deletions do not use WCU / RCU
- TTL is a background task operated by the DynamoDB service itself
- Helps reduce storage and manage the table size over time
- Helps adhere to regulatory norms
- TTL is enabled per row (you define a TTL column, and add a date there)
- DynamoDB typically deletes expired items within 48 hours of expiration
- Deleted items due to TTL are also deleted in GSI / LSI
- DynamoDB Streams can help recover expired items
### DynamoDB – Security & Other Features
- Security:
- VPC Endpoints available to access DynamoDB without internet
- Access fully controlled by IAM
- Encryption at rest using KMS
- Encryption in transit using SSL / TLS
- Backup and Restore feature available
- Point in time restore like RDS
- No performance impact
- Global Tables
- Multi region, fully replicated, high performance
- Amazon Database Migration Service (DMS) can be used to migrate to DynamoDB (from Mongo, Oracle, MySQL, S3, etc…)
- You can launch a local DynamoDB on your computer for development purposes
### DynamoDB – Storing large objects
- Max size of an item in DynamoDB = 400 KB
- For large objects, store them in S3 and reference them in DynamoDB Amazon S3
### AWS ElastiCache Overview
- The same way RDS is to get managed Relational Databases…
- ElastiCache is to get managed Redis or Memcached
- Caches are in-memory databases with really high performance, low latency
- Helps reduce load off of databases for read intensive workloads
- Helps make your application stateless
- Write Scaling using sharding
- Read Scaling using Read Replicas
- Multi AZ with Failover Capability
- AWS takes care of OS maintenance / patching, optimizations, setup, configuration, monitoring, failure recovery and backups
### Redis Overview
- Redis is an in-memory key-value store
- Super low latency (sub ms)
- Cache survive reboots by default (it’s called persistence)
- Great to host
- User sessions
- Leaderboard (for gaming)
- Distributed states
- Relieve pressure on databases (such as RDS)
- Pub / Sub capability for messaging
- Multi AZ with Automatic Failover for disaster recovery if you don’t want to lose your cache data
- Support for Read Replicas
### Memcached Overview
- Memcached is an in-memory object store
- Cache doesn’t survive reboots
- Use cases:
- Quick retrieval of objects from memory
- Cache often accessed objects
- Overall, Redis has largely grown in popularity and has better feature sets than Memcached.
- I would personally only use Redis for caching needs.
