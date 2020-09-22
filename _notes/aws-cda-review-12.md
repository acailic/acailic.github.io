---
title: AWS Integration & Messaging
layout: post
tags: [aws, cda, sns, sqs, kinesis]
date: 2020-09-17
---

### Intro
1) Synchronous communicaions
(applicaion to applicaion)
2) Asynchronous / Event based
(applicaion to queue to applicaion)
-Synchronous between applications can be problematic if there are
sudden spikes of traffic
• What if you need to suddenly encode 1000 videos but usually it’s 10?
- In that case, it’s better to decouple your applications,
• using SQS: queue model
• using SNS: pub/sub model
• using Kinesis: real-time streaming model

### Amazon SQS – Standard Queue
- Oldest offering (over 10 years old)
- Fully managed service, used to decouple applications
- Attributes:
• Unlimited throughput, unlimited number of messages in queue
• Shortlived, Default retention of messages: 4 days, maximum of 14 days
• Low latency (<10 ms on publish and receive)
• Limitation of 256KB per message sent
- Can have duplicate messages (at least once delivery, occasionally)
- Can have out of order messages (best effort ordering)
#### SQS – Producing Messages
- Produced to SQS using the SDK (SendMessage API)
- The message is persisted in SQS until a consumer deletes it
- Message retention: default 4 days, up to 14 days
- Example: send an order to be processed
• Order id
• Customer id
• Any attributes you want
- SQS standard: unlimited throughput
#### SQS – Consuming Messages
- Consumers (running on EC2 instances, servers, or AWS Lambda)…
- Poll SQS for messages (receive up to 10 messages at a time)
- Process the messages (example: insert the message into an RDS database)
- Delete the messages using the DeleteMessage API
#### SQS – Multiple EC2 Instances Consumers
-Consumers receive and process
messages in parallel
- At least once delivery
- Best-effort message ordering
- Consumers delete messages
after processing them
- We can scale consumers
horizontally to improve
throughput of processing
#### SQS with Auto Scaling Group (ASG)
- ASG scalling on queue length like a CloudWatchMetric. CloudWatch Alarm
#### SQS to decouple between application tiers
- Three layers: FE + Queue + BE
#### Amazon SQS - Security
- Encryption:
• In-flight encryption using HTTPS API
• At-rest encryption using KMS keys
• Client-side encryption if the client wants to perform encryption/decryption itself ( not out of box)
- Access Controls: IAM policies to regulate access to the SQS API
- SQS Access Policies (similar to S3 bucket policies)
• Useful for cross-account access to SQS queues
• Useful for allowing other services (SNS, S3…) to write to an SQS queue
#### SQS – Message Visibility Timeout
• After a message is polled by a consumer, it becomes invisible to other consumers
• By default, the “message visibility timeout” is 30 seconds
• That means the message has 30 seconds to be processed
• After the message visibility timeout is over, the message is “visible” in SQS
If a message is not processed within the visibility timeout, it will be processed twice
• A consumer could call the ChangeMessageVisibility API to get more time
• If visibility timeout is high (hours), and consumer crashes, re-processing will take time
• If visibility timeout is too low (seconds), we may get duplicates
#### Amazon SQS – Dead Letter Queue
• If a consumer fails to process a message within the
Visibility Timeout…
the message goes back to the queue!
• We can set a threshold of how many times a message
can go back to the queue
• After the MaximumReceives threshold is exceeded, the
message goes into a dead letter queue (DLQ)
• Useful for debugging!
• Make sure to process the messages in the DLQ before
they expire:
• Good to set a retention of 14 days in the DLQ
#### Amazon SQS – Delay Queue
• Delay a message (consumers don’t see it immediately) up to 15 minutes
• Default is 0 seconds (message is available right away)
• Can set a default at queue level
• Can override the default on send using the DelaySeconds parameter
#### Amazon SQS - Long Polling
• When a consumer requests messages from the
queue, it can optionally “wait” for messages to
arrive if there are none in the queue
• This is called Long Polling
• LongPolling decreases the number of API calls
made to SQS while increasing the efficiency and
latency of your application.
• The wait time can be between 1 sec to 20 sec
(20 sec preferable)
• Long Polling is preferable to Short Polling
• Long polling can be enabled at the queue level or at the API level using WaitTimeSeconds
#### SQS Extended Client
• Message size limit is 256KB, how to send large messages, e.g. 1GB?
• Using the SQS Extended Client (Java Library)
#### SQS – Must know API
• CreateQueue (MessageRetentionPeriod), DeleteQueue
• PurgeQueue: delete all the messages in queue
• SendMessage (DelaySeconds), ReceiveMessage, DeleteMessage
• ReceiveMessageWaitTimeSeconds: Long Polling
• ChangeMessageVisibility: change the message timeout
• Batch APIs for SendMessage, DeleteMessage, ChangeMessageVisibility
helps decrease your costs
#### Amazon SQS – FIFO Queue
Limited throughput: 300 msg/s without batching, 3000 msg/s with
• Exactly-once send capability (by removing duplicates)
• Messages are processed in order by the consumer
#### SQS FIFO – Deduplication
• FIFO = First In First Out (ordering of messages in the queue)
• De-duplication interval is 5 minutes
- Two de-duplication methods:
• Content-based deduplication: will do a SHA-256 hash of the message body
• Explicitly provide a Message Deduplication ID
#### SQS FIFO – Message Grouping
• If you specify the same value of MessageGroupID in an SQS FIFO queue,
you can only have one consumer, and all the messages are in order
• To get ordering at the level of a subset of messages, specify different values
for MessageGroupID
• Messages that share a common Message Group ID will be in order within the group
• Each Group ID can have a different consumer (parallel processing!)
• Ordering across groups is not guaranteed

## Amazon SNS
• What if you want to send one message to many receivers?
- Pub/Sub pattern. SNS topic beetween integrations
### Amazon SNS
- The “event producer” only sends message to one SNS topic
- As many “event receivers” (subscriptions) as we want to listen to the SNS topic notifications
- Each subscriber to the topic will get all the messages (note: new feature to filter messages)
- Up to 10,000,000 subscriptions per topic
- 100,000 topics limit
- Subscribers can be:
• SQS
• HTTP / HTTPS (with delivery retries – how many times)
• Lambda
• Emails
• SMS messages
• Mobile Notifications
### SNS integrates with a lot of AWS services
- Many AWS services can send data directly to SNS for notifications
- CloudWatch (for alarms)
- Auto Scaling Groups notifications
- Amazon S3 (on bucket events)
- CloudFormation (upon state changes => failed to build, etc)
- center of aws
### AWS SNS – How to publish
- Topic Publish (using the SDK)
• Create a topic
• Create a subscription (or many)
• Publish to the topic
- Direct Publish (for mobile apps SDK)
• Create a platform application
• Create a platform endpoint
• Publish to the platform endpoint
• Works with Google GCM, Apple APNS, Amazon ADM…
### Amazon SNS – Security
- Encryption:
• In-flight encryption using HTTPS API
• At-rest encryption using KMS keys
• Client-side encryption if the client wants to perform encryption/decryption itself
- Access Controls: IAM policies to regulate access to the SNS API
• SNS Access Policies (similar to S3 bucket policies)
- Useful for cross-account access to SNS topics
• Useful for allowing other services ( S3…) to write to an SNS topic
### SNS + SQS: Fan Out
• Push once in SNS, receive in all SQS queues that are subscribers
• Fully decoupled, no data loss
• SQS allows for: data persistence, delayed processing and retries of work
• Ability to add more SQS subscribers over time
• Make sure your SQS queue access policy allows for SNS to write
• SNS cannot send messages to SQS FIFO queues (AWS limitation)
### Stephane Maarek www.datacumulus.com
Application: S3 Events to multiple queues
• For the same combination of: event type (e.g. object create) and prefix
(e.g. images/) you can only have one S3 Event rule
• If you want to send the same S3 event to many SQS queues, use fan-out

## AWS Kinesis Overview
• Kinesis is a managed alternative to Apache Kafka
• Great for application logs, metrics, IoT, clickstreams
• Great for “real-time” big data
• Great for streaming processing frameworks (Spark, NiFi, etc…)
• Data is automatically replicated to 3 AZ
• Kinesis Streams: low latency streaming ingest at scale
• Kinesis Analytics: perform real-time analytics on streams using SQL
• Kinesis Firehose: load streams into S3, Redshift, ElasticSearch…
#### Kinesis Streams Overview
• Streams are divided in ordered Shards / Partitions
• Data retention is 1 day by default, can go up to 7 days
• Ability to reprocess / replay data
• Multiple applications can consume the same stream
• Real-time processing with scale of throughput
• Once data is inserted in Kinesis, it can’t be deleted (immutability)
#### Kinesis Streams Shards
• One stream is made of many different shards
• 1MB/s or 1000 messages/s at write PER SHARD
• 2MB/s at read PER SHARD
• Billing is per shard provisioned, can have as many shards as you want
• Batching available or per message calls.
• The number of shards can evolve over
#### AWS Kinesis API – Put records
• PutRecord API + Partition key that gets
hashed
• The same key goes to the same partition
(helps with ordering for a specific key)
• Messages sent get a “sequence number”
• Choose a partition key that is highly
distributed (helps prevent “hot partition”)
• user_id if many users
• Not country_id if 90% of the users are in one
country
• Use Batching with PutRecords to reduce
costs and increase throughput
• ProvisionedThroughputExceeded if we go
over the limits
• Can use CLI, AWS SDK, or producer
libraries from various frameworks
#### AWS Kinesis API – Exceptions
• ProvisionedThroughputExceeded Exceptions
• Happens when sending more data (exceeding MB/s or TPS for any shard)
• Make sure you don’t have a hot shard (such as your partition key is bad and too
much data goes to that partition)
• Solution:
• Retries with backoff
• Increase shards (scaling)
• Ensure your partition key is a good one
#### AWS Kinesis API – Consumers
• Can use a normal consumer (CLI,
SDK, etc…)
• Can use Kinesis Client Library (in
Java, Node, Python, Ruby, .Net)
• KCL uses DynamoDB to checkpoint
offsets
• KCL uses DynamoDB to track other
workers and share
####  Kinesis KCL in Depth
• Kinesis Client Library (KCL) is Java library that helps read record from a
Kinesis Streams with distributed applications sharing the read workload
• Rule: each shard is be read by only one KCL instance
• Means 4 shards = max 4 KCL instances
• Means 6 shards = max 6 KCL instances
• Progress is checkpointed into DynamoDB (need IAM access)
• KCL can run on EC2, Elastic Beanstalk, on Premise Application
• Records are read in order at the shard level
#### Kinesis Security
• Control access / authorization using IAM policies
• Encryption in flight using HTTPS endpoints
• Encryption at rest using KMS
• Possibility to encrypt / decrypt data client side (harder)
• VPC Endpoints available for Kinesis to access within VPC
####  AWS Kinesis Data Analytics
• Perform real-time analytics on Kinesis Streams using SQL
• Kinesis Data Analytics:
• Auto Scaling
• Managed: no servers to provision
• Continuous: real time
• Pay for actual consumption rate
• Can create streams out of the real-time queries
#### AWS Kinesis Firehose
• Fully Managed Service, no administration
• Near Real Time (60 seconds latency)
• Load data into Redshift / Amazon S3 / ElasticSearch / Splunk
• Automatic scaling
• Support many data format (pay for conversion)
• Pay for the amount of data going through Firehose
#### Ordering data into Kinesis
• Imagine you have 100 trucks
(truck_1, truck_2, … truck_100) on
the road sending their GPS positions
regularly into AWS.
• You want to consume the data in
order for each truck, so that you can
track their movement accurately.
• How should you send that data into
Kinesis?
• Answer: send using a “Partition Key”
value of the “truck_id”
• The same key will always go to the
same shard
#### Ordering data into SQS 
#### Kinesis vs SQS ordering
• Let’s assume 100 trucks, 5 kinesis shards, 1 SQS FIFO
• Kinesis Data Streams:
• On average you’ll have 20 trucks per shard
• Trucks will have their data ordered within each shard
• The maximum amount of consumers in parallel we can have is 5
• Can receive up to 5 MB/s of data
• SQS FIFO
• You only have one SQS FIFO queue
• You will have 100 Group ID
• You can have up to 100 Consumers (due to the 100 Group ID)
• You have up to 300 messages per second (or 3000 if using batching)
#### SQS vs SNS vs Kinesis
SQS:
• Consumer “pull data”
• Data is deleted after being
consumed
• Can have as many workers
(consumers) as we want
• No need to provision
throughput
• No ordering guarantee
(except FIFO queues)
• Individual message delay
capability
SNS:
• Push data to many
subscribers
• Up to 10,000,000
subscribers
• Data is not persisted (lost if
not delivered)
• Pub/Sub
• Up to 100,000 topics
• No need to provision
throughput
• Integrates with SQS for fanout
architecture pattern
Kinesis:
• Consumers “pull data”
• As many consumers as we
want
• Possibility to replay data
• Meant for real-time big data,
analytics and ETL
• Ordering at the shard level
• Data expires after X days
• Must provision throughput
