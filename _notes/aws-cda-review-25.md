---
title: AWS DynamoDB
layout: post
tags: [aws, cda, dynamo, db]
date: 2020-09-17
---

### AWS DynamoDB
#### Traditional Architecture
- Traditional applications leverage RDBMS databases
- These databases have the SQL query language
- Strong requirements about how the data should be modeled
- Ability to do join, aggregations, computations
- Vertical scaling (means usually getting a more powerful CPU / RAM / IO)
#### NoSQL databases
- NoSQL databases are non-relational databases and are distributed
- NoSQL databases include MongoDB, DynamoDB, etc.
- NoSQL databases do not support join
- All the data that is needed for a query is present in one row
- NoSQL databases don’t perform aggregations such as “SUM”
- NoSQL databases scale horizontally
- There’s no “right or wrong” for NoSQL vs SQL, they just require to
model the data differently and think about user queries differently
### DynamoDB
- Fully Managed, Highly available with replication across 3 AZ
- NoSQL database - not a relational database
- Scales to massive workloads, distributed database
- Millions of requests per seconds, trillions of row, 100s of TB of storage
- Fast and consistent in performance (low latency on retrieval)
- Integrated with IAM for security, authorization and administration
- Enables event driven programming with DynamoDB Streams
• Low cost and auto scaling capabilities
### DynamoDB- Basics
- DynamoDB is made of tables
- Each table has a primary key (must be decided at creation time)
- Each table can have an infinite number of items (= rows)
- Each item has attributes (can be added over time – can be null)
- Maximum size of a item is 400KB
- Data types supported are:
• Scalar Types: String, Number, Binary, Boolean, Null
• Document Types: List, Map
• Set Types: String Set, Number Set, Binary Set
- Nested stuff
### DynamoDB – Primary Keys
- Option 1: Partition key only (HASH)
- Partition key must be unique for each
item
- Partition key must be “diverse” so
that the data is distributed
• Example: user_id for a users table
• Option 2: Partition key + Sort Key
• The combination must be unique
• Data is grouped by partition key
• Sort key == range key
• Example: users-games table
• user_id for the partition key
• game_id for the sort key
### DynamoDB – Provisioned Throughput
- Table must have provisioned read and write capacity units
- Read Capacity Units (RCU): throughput for reads
- Write Capacity Units (WCU): throughput for writes
- Option to setup auto-scaling of throughput to meet demand
- Throughput can be exceeded temporarily using “burst credit”
- If burst credit are empty, you’ll get a “ProvisionedThroughputException”.
- It’s then advised to do an exponential back-off retry
#### DynamoDB – Write Capacity Units
- One write capacity unit represents one write per second for an item up
to 1 KB in size.
- If the items are larger than 1 KB, more WCU are consumed
- 10 obje per sec for 2KB each= 2*10WCU
- 6 obj per sec of 4.5KB, 6*5 rounded, 30WCU 
#### Strongly Consistent Read vs Eventually Consistent Read
- Eventually Consistent Read: If we read
just after a write, it’s possible we’ll get
unexpected response because of
replication
- Strongly Consistent Read: If we read just
after a write, we will get the correct data
- By default: DynamoDB uses Eventually
Consistent Reads, but GetItem, Query &
Scan provide a “ConsistentRead”
parameter you can set to True
#### DynamoDB – Read Capacity Units
- One read capacity unit represents one strongly consistent read per second, or
two eventually consistent reads per second, for an item up to 4 KB in size.
- If the items are larger than 4 KB, more RCU are consumed
- 10 strongly conistent reads pers sec of 4KB = 10*4/4=10RCU
- 16 eventualy conistent reads pers sec of 12KB = (16/2)*(12/4)=24RCU
- 10 strongly conistent reads pers sec of 6KB = 10*8/4=20RCU
#### DynamoDB – Partitions Internal
- Data is divided in partitions
• Partition keys go through a hashing algorithm to know to which
partition they go to
- To compute the number of partitions:
• By capacity: (TOTAL RCU / 3000) + (TOTAL WCU / 1000)
• By size: Total Size / 10 GB
• Total partitions = CEILING(MAX(Capacity, Size))
- WCU and RCU are spread evenly between partitions
####  DynamoDB - Throttling
- If we exceed our RCU or WCU, we get ProvisionedThroughputExceededExceptions
- Reasons:
• Hot keys: one partition key is being read too many times (popular item for ex)
• Hot partitions:
• Very large items: remember RCU and WCU depends on size of items
- Solutions:
• Exponential back-off when exception is encountered (already in SDK)
• Distribute partition keys as much as possible
- If RCU issue, we can use DynamoDB Accelerator (DAX), its like a cache
#### DynamoDB – Writing Data
- PutItem - Write data to DynamoDB (create data or full replace)
• Consumes WCU
- UpdateItem – Update data in DynamoDB (partial update of attributes)
• Possibility to use Atomic Counters and increase them
- Conditional Writes:
• Accept a write / update only if conditions are respected, otherwise reject
• Helps with concurrent access to items
• No performance impact
#### DynamoDB – Deleting Data
- DeleteItem
• Delete an individual row
- Ability to perform a conditional delete
- DeleteTable
• Delete a whole table and all its items
• Much quicker deletion than calling DeleteItem on all items
#### DynamoDB – Batching Writes
- BatchWriteItem
• Up to 25 PutItem and / or DeleteItem in one call
• Up to 16 MB of data written
• Up to 400 KB of data per item
- Batching allows you to save in latency by reducing the number of API
calls done against DynamoDB
- Operations are done in parallel for better efficiency
- It’s possible for part of a batch to fail, in which case we have the try the
failed items (using exponential back-off algorithm)
#### DynamoDB – Reading Data
- GetItem:
• Read based on Primary key
• Primary Key = HASH or HASH-RANGE
• Eventually consistent read by default
• Option to use strongly consistent reads (more RCU - might take longer)
• ProjectionExpression can be specified to include only certain attributes
- BatchGetItem:
• Up to 100 items
• Up to 16 MB of data
• Items are retrieved in parallel to minimize latency
#### DynamoDB – Query
- Query returns items based on:
• PartitionKey value (must be = operator)
• SortKey value (=, <, <=, >, >=, Between, Begin) – optional
• FilterExpression to further filter (client side filtering)
- Returns:
• Up to 1 MB of data
• Or number of items specified in Limit
- Able to do pagination on the results
- Can query table, a local secondary index, or a global secondary index
####  DynamoDB - Scan
- Scan the entire table and then filter out data (inefficient)
- Returns up to 1 MB of data – use pagination to keep on reading
- Consumes a lot of RCU
- Limit impact using Limit or reduce the size of the result and pause
- For faster performance, use parallel scans:
• Multiple instances scan multiple partitions at the same time
• Increases the throughput and RCU consumed
• Limit the impact of parallel scans just like you would for Scans
- Can use a ProjectionExpression + FilterExpression (no change to RCU)
#### DynamoDB – LSI (Local Secondary Index)
- Alternate range key for your table, local to the hash key
- Up to five local secondary indexes per table.
- The sort key consists of exactly one scalar attribute.
- The attribute that you choose must be a scalar String, Number, or Binary
- LSI must be defined at table creation time
#### DynamoDB – GSI (Global Secondary Index)
- To speed up queries on non-key attributes, use a Global Secondary Index
- GSI = partition key + optional sort key
- The index is a new “table” and we can project attributes on it
• The partition key and sort key of the original table are always projected (KEYS_ONLY)
• Can specify extra attributes to project (INCLUDE)
• Can use all attributes from main table (ALL)
- Must define RCU / WCU for the index
- Possibility to add / modify GSI (not LSI)
####  DynamoDB Indexes and Throttling
- GSI:
• If the writes are throttled on the GSI, then the main table will be throttled!
• Even if the WCU on the main tables are fine
• Choose your GSI partition key carefully!
• Assign your WCU capacity carefully!
- LSI:
• Uses the WCU and RCU of the main table
• No special throttling considerations
#### DynamoDB Concurrency
- DynamoDB has a feature called “Conditional Update / Delete”
- That means that you can ensure an item hasn’t changed before altering it
- That makes DynamoDB an optimistic locking / concurrency database
#### DynamoDB - DAX
- DAX = DynamoDB Accelerator, like cache
- Seamless cache for DynamoDB, no application rewrite
- Writes go through DAX to DynamoDB
- Micro second latency for cached reads & queries
- Solves the Hot Key problem (too many reads)
- 5 minutes TTL for cache by default
- Up to 10 nodes in the cluster
- Multi AZ (3 nodes minimum recommended for
production)
• Secure (Encryption at rest with KMS, VPC, IAM,
CloudTrail…)
### DynamoDB – DAX vs ElastiCache
- DAX individual object cache and elastic cache store as well aggregation result
####  DynamoDB Streams
- Changes in DynamoDB (Create, Update, Delete) can end up in a DynamoDB Stream
- This stream can be read by AWS Lambda & EC2 instances, and we can then do:
• React to changes in real time (welcome email to new users)
• Analytics
• Create derivative tables / views
• Insert into ElasticSearch
- Could implement cross region replication using Streams; now its embedded
- Stream has 24 hours of data retention
- Choose the information that will be written to the stream whenever
the data in the table is modified:
• KEYS_ONLY — Only the key attributes of the modified item.
• NEW_IMAGE —The entire item, as it appears after it was modified.
• OLD_IMAGE —The entire item, as it appeared before it was modified.
• NEW_AND_OLD_IMAGES — Both the new and the old images of the item. useful but expensive
- DynamoDB Streams are made of shards, just like Kinesis Data Streams
- You don’t provision shards, this is automated by AWS
- Records are not retroactively populated in a stream after enabling it
#### DynamoDB Streams & Lambda
- You need to define an EventSource Mapping to read from
a DynamoDB Streams
- You need to ensure the Lambda function has the appropriate permissions
- Your Lambda function is invoked synchronously
#### DynamoDB - TTL (Time to Live)
- TTL = automatically delete an item after an expiry date / time
- TTL is provided at no extra cost, deletions do not use WCU / RCU
- TTL is a background task operated by the DynamoDB service itself
- Helps reduce storage and manage the table size over time
- Helps adhere to regulatory norms
- TTL is enabled per row (you define a TTL column, and add a date there)
- DynamoDB typically deletes expired items within 48 hours of expiration
- Deleted items due to TTL are also deleted in GSI / LSI
- DynamoDB Streams can help recover expired items
##### DynamoDB CLI – Good to Know
- --projection-expression : attributes to retrieve
- --filter-expression : filter results
- General CLI pagination options including DynamoDB / S3:
- Optimization:
• --page-size : full dataset is still received but each API call will request less data (helps avoid
timeouts)
- Pagination:
• --max-items : max number of results returned by the CLI. Returns NextToken
• --starting-token: specify the last received NextToken to keep on reading
#### DynamoDB Transactions
- New feature from November 2018
- Transaction = Ability to Create / Update / Delete multiple rows in
different tables at the same time
- It’s an “all or nothing” type of operation.
- Write Modes: Standard, Transactional
- Read Modes: Eventual Consistency, Strong Consistency, Transactional
- Consume 2x of WCU / RCU
####  DynamoDB as Session State Cache
- It’s common to use DynamoDB to store session state
- vs ElastiCache:
• ElastiCache is in-memory, but DynamoDB is serverless
• Both are key/value stores
• question do you want serverless and automatic scalling
- vs EFS:
• EFS must be attached to EC2 instances as a network drive
- vs EBS & Instance Store:
• EBS & Instance Store can only be used for local caching, not shared caching
- vs S3:
• S3 is higher latency, and not meant for small objects
##### DynamoDB Write Sharding
- Imagine we have a voting application with two candidates, candidate A and
candidate B.
- If we use a partition key of candidate_id, we will run into partitions issues, as we
only have two partitions
- Solution: add a suffix (usually random suffix, sometimes calculated suffix); Candidate_A-1;
#### DynamoDB – Write Types
- Concurrent Writes
- Conditional Writes
- Atomic Writes
- Batch Writes
#### DynamoDB - Large Objects Pattern
- upload to s3 big file and use metadata from file
#### DynamoDB - Indexing s3
- Upload metadata of files into DynamoDB and use api for search
##### DynamoDB Operations
- Table Cleanup:
• Option 1: Scan + Delete => very slow,expensive, consumes RCU & WCU
• Option 2: Drop Table + Recreate table =>fast, cheap, efficient
- Copying a DynamoDB Table:
• Option 1: Use AWS DataPipeline (uses EMR)
• Option 2: Create a backup and restore the
backup into a new table name (can take sometime)
• Option 3: Scan + Write => write own code
##### DynamoDB – Security & Other Features
- Security:
• VPC Endpoints available to access DynamoDB without internet
• Access fully controlled by IAM
• Encryption at rest using KMS
• Encryption in transit using SSL / TLS
- Backup and Restore feature available
• Point in time restore like RDS
• No performance impact
- Global Tables
• Multi region, fully replicated, high performance
- Amazon DMS can be used to migrate to DynamoDB (from Mongo, Oracle, MySQL,
S3, etc…)
- You can launch a local DynamoDB on your computer for development purposes
### Questions
- We have to provision the instance type for our DynamoDB database?-no
- We have to provision read and write capacity units for our DynamoDB tables ?-yes
- DynamoDB tables scale?-Horizontally
- If my primary key is a combination of partition key and sort key, then?-Partition+SortKey must be unique
- You are designing a blog post table. Which column will give us the best partition key for optimal distribution?-blog_id 
- You are writing item of 8 KB in size at the rate of 12 per seconds. What WCU do you need?-96
- You are doing strongly consistent read of 10 KB items at the rate of 10 per second. What RCU do you need?-10 KB gets rounded to 12 KB, divided by 4KB = 3, times 10 per second = 30
- You are doing 12 eventually consistent reads per second, and each item has a size of 16 KB. What RCU do you need?-we can do 2 eventually consistent reads per seconds for items of 4 KB with 1 RCU
- We are getting a ProvisionedThroughputExceededExceptions but after checking the metrics, we see we haven't exceeded the total RCU we had provisioned. What happened?- Hot partition/hot key.remember RCU and WCU are spread across all partitions 
- You are about to enter the Christmas sale and you know a few items in your website are very popular and will be read often. Last year you had a ProvisionedThroughputExceededException. What should you do this year?-Create a DAX cluster
- How can you select the attributes to retrieve in the response while using the GetItem DynamoDB CLI?-ProjectionExpression
- You want to delete all the data in your table. What's the best way of doing it?-DeleteTable and then CreateTable
- You want to increase the performance of your scan operation. What should you do?- Use parallel scans
- You want to use Query equal operation on a non key attribute. How can you do it?-Create a globall secondary index
- You would like to have query for a non key attribute for the >= predicate while keeping the same partition key. You should-Create Local Secondary index
- You would like to react in real-time to users de-activating their account and send them an email to try to bring them back. The best way of doing it is to ?-Integrate Lambda with Dynamo Stream
- Which concurrency model can be implemented with DynamoDB?-Optimistic Locking
- Which feature of DynamoDB allows it to achieve Optimistic Locking?-Conditional Writes
- You have created a DynamoDB table to support your application and provisioned RCU and WCU to it, so that your application has been running for over a year now without any throttling issues. Your application now requires a second type of queries over your table and as such, you have decided to use the existing LSI and created a GSI to support that use case. One month after having implemented such indexes, it seems your table is experiencing throttling. Upon looking at the table's metrics, it seems the RCU and WCU provisioned are still sufficient. What's happening?-The GSI is throttling so you need to provision more RCU and WCU to the GSI.GSI have an independent amount of RCU and WCU and if they are throttled due to insufficient capacity, then the main table will also be throttled
- You would like to have DynamoDB automatically delete old data for you. What should you use?-Use TTL
- Which of the following CLI options will allow you to retrieve a subset of the attributes coming from a DynamoDB scan?-Project expression
- You would like to paginate the results of a DynamoDB scan in order to minimize the amount of RCU that you will use for that CLI command. Which CLI options should you use?---max-items & --starting-token
- You are implementing a banking application in which you need to update the Exchanges DynamoDB table and the AccountBalance DynamoDB table at the same time or not at all. Which DynamoDB feature should you use?-DynamoDB Transactions
