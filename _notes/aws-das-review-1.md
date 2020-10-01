---
title: AWS  Kinesis 
layout: post
tags: [aws, sda, kineisis]
date: 2020-09-30
---
###  Collection Introduction
- Real Time Immediate actions
• Kinesis Data Streams (KDS)
•Simple Queue Service (SQS)
•Internet of Things (IoT)
- Near real time Reactive actions
•Kinesis Data Firehose (KDF)
•Database Migration Service (DMS)
- Batch Historical Analysis
•Snowball
•Data Pipeline

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
-
Producer:
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
