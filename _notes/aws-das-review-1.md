---
title: AWS DAS Kinesis 
layout: post
tags: [aws, das, kineisis]
date: 2020-10-01
---
###  Collection Introduction
- Real Time Immediate actions
• Kinesis Data Streams (KDS)
• Simple Queue Service (SQS)
• Internet of Things (IoT)
- Near real time Reactive actions
• Kinesis Data Firehose (KDF)
• Database Migration Service (DMS)
- Batch Historical Analysis
• Snowball
• Data Pipeline

###  AWS Kinesis Overview
• Kinesis is a managed alternative to Apache Kafka
• Great for application logs, metrics, IoT, clickstreams
• Great for “real-time” big data
• Great for streaming processing frameworks (Spark, NiFi, etc…)
• Data is automatically replicated synchronously to 3 AZ
• Kinesis Streams: low latency streaming ingest at scale
• Kinesis Analytics: perform real-time analytics on streams using
SQL
• Kinesis Firehose: load streams into S3, Redshift, ElasticSearch &
Splunk

#### Kinesis Streams Overview
- Streams are divided in ordered Shards / Partitions
- Data retention is 24 hours by default, can go up to 7 days
- Ability to reprocess / replay data
- Multiple applications can consume the same stream
- Real time processing with scale of throughput
- Once data is inserted in Kinesis, it can’t be deleted (immutability)

#### Kinesis Streams Shards
- One stream is made of many different shards
- Billing is per shard provisioned, can have as many shards as
you want
- Batching available or per message calls.
- The number of shards can evolve over time ( reshard /
- Records are ordered per shard

#### Kinesis Streams Records
- Data Blob: data being sent, serialized
as bytes . Up to 1 MB. Can represent
anything
- Record Key:
• sent alongside a record, helps to group
records in Shards. Same key = Same
shard.
• Use a highly distributed key to avoid the
“hot partition” problem
- Sequence number: Unique identifier
for each records put in shards. Added
by Kinesis after ingestion
#### Kinesis Data Streams Limits to know
-Producer:
• 1MB/s or 1000 messages/s at write PER SHARD
• ProvisionedThroughputException  otherwise
- Consumer Classic:
• 2MB/s at read PER SHARD across all consumers
• 5 API calls per second PER SHARD across all consumers
- Consumer Enhanced Fan Out:
• 2MB/s at read PER SHARD, PER ENHANCED CONSUMER
• No API calls needed (push model)
- Data Retention:
• 24 hours data retention by default
• Can be extended to 7 days
### Kinesis Producers
- Kinesis SDK
- Kinesis Producer
Library (KPL)
- Kinesis Agent
- 3rd party libraries:Spark, Log4J Appenders, Flume,Kafka Connect, Nifi ..
### AWS Kinesis API-Exceptions
- ProvisionedThroughputExceeded Exceptions
• Happens when sending more data (exceeding MB/s or TPS for any
shard)
• Make sure you don’t have a hot shard (such as your partition key is bad
and too much data goes to that partition)
-Solution:
• Retries with backoff
• Increase shards (scaling)
• Ensure your partition key is a good one
### Kinesis Producer Library (KPL)
- Easy to use and highly configurable C++ / Java library
- Used for building high performance, long running producers
- Automated and configurable retry mechanism
- Synchronous or Asynchronous API (better performance for async
- Submits metrics to CloudWatch for monitoring
- Batching (both turned on by default) increase throughput, decrease
cost:
- Collect Records and Write to multiple shards in the same PutRecords API call
- Aggregate increased latency
- Capability to store multiple records in one record (go over 1000 records per second limit)
- Increase payload size and improve throughput (maximize 1MB/s limit)
- Compression must be implemented by the user
- KPL Records must be de coded with KCL or special helper library
- We can influence the batching efficiency by introducing some
delay with RecordMaxBufferedTime (default
### Kinesis Agent
- Monitor Log files and sends them to Kinesis Data Streams
-Java based agent, built on top of KPL
- Install in Linux based server environments
- Features:
• Write from multiple directories and write to multiple streams
• Routing feature based on directory / log file
• Pre process data before sending to streams (single line, csv to json , log to
json
• The agent handles file rotation, checkpointing, and retry upon failures
• Emits metrics to CloudWatch for monitoring

## Kinesis Consumers - Classic
- Kinesis SDK
- Kinesis Client Library (KCL)
- Kinesis Connector Library
- 3rd party libraries: Spark,
Log4J Appenders, Flume,
Kafka Connect…
- Kinesis Firehose
- AWS Lambda
- (Kinesis Consumer Enhanced
Fan-Out discussed in the next
lecture)
### Kinesis Consumer SDK GetRecords
- Classic Kinesis Records are
polled by consumers from a
shard
- Each shard has 2 MB total
aggregate throughput
-GetRecords returns up to 10MB
of data (then throttle for 5
seconds) or up to 10000 records
- Maximum of 5 GetRecords API
calls per shard per second =
200ms latency
- If 5 consumers application
consume from the same shard,
means every consumer can poll
once a second and receive less
than 400 KB/s
### Kinesis Client Library (KCL)
- Java first library but exists for other
languages too (Golang, Python, Ruby, Node,
.NET
- Read records from Kinesis produced with the
KPL (de aggregation)
- Share multiple shards with multiple
consumers in one “group”, shard discovery
- Checkpointing feature to resume progress
- Leverages DynamoDB for coordination and
checkpointing (one row per shard)
- Make sure you provision enough WCU / RCU
- Or use On Demand for DynamoDB
- Otherwise DynamoDB may slow down KCL
- Record processors will process the data
### Kinesis Connector Library
- Older Java library (2016),
leverages the KCL library
- Write data to:
• Amazon S3
• DynamoDB
• Redshift
• ElasticSearch
- Kinesis Firehose replaces the
Connector Library for a few of
these targets, Lambda for the
others
### AWS Lambda sourcing from Kinesis
- AWS Lambda can source records from Kinesis Data Streams
- Lambda consumer has a library to de aggregate record from the
KPL
- Lambda can be used to run lightweight ETL to:
• Amazon S3
• DynamoDB
• Redshift
• ElasticSearch
• Anywhere you want
- Lambda can be used to trigger notifications / send emails in real time
- Lambda has a configurable batch size (more in Lambda section)
### Kinesis Enhanced Fan Out
- New game changing feature from
August 2018.
- Works with KCL 2.0 and AWS
Lambda (Nov 2018)
- Each Consumer get 2 MB/s of
provisioned throughput per shard
- That means 20 consumers will get
40MB/s per shard aggregated
- No more 2 MB/s limit!
- Enhanced Fan Out: Kinesis pushes
data to consumers over HTTP/2
- Reduce latency (~70 ms)
### Enhanced FanOut vs Standard Consumers
- Standard consumers:
• Low number of consuming applications (1,2,3…)
• Can tolerate ~200 ms latency
• Minimize cost
- Enhanced Fan Out Consumers:
• Multiple Consumer applications for the same Stream
• Low Latency requirements ~70ms
• Higher costs (see Kinesis pricing page)
• Default limit of 5 consumers using enhanced fan out per data stream
