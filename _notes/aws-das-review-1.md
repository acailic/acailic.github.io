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
### Kinesis Operations-Adding Shards
- Also called “Shard Splitting”
- Can be used to increase the Stream capacity (1 MB/s data in per
shard)
- Can be used to divide a “hot shard”
- The old shard is closed and will be deleted once the data is expired
### Kinesis Operations Merging Shards
- Decrease the Stream capacity and save costs
- Can be used to group two shards with low traffic
- Old shards are closed and deleted based on data expiration
### Kinesis Operations Auto Scaling
- Auto Scaling is not a native
feature of Kinesis
- The API call to change the
number of shards is
UpdateShardCount
- We can implement Auto Scaling
with AWS Lambda
### Kinesis Scaling Limitations
- Resharding cannot be done in parallel. Plan capacity in advance
- You can only perform one resharding operation at a time and it takes a few
s econds
- For1000 shards, it takes 30K seconds (8.3 hours) to double the shards to
2000
- You can’t do the following:
• Scale more than twice for each rolling 24 hour period for each stream
• Scale up to more than double your current shard count for a stream
• Scale down below half your current shard count for a stream
• Scale up to more than 500 shards in a stream
• Scale a stream with more than 500 shards down unless the result is fewer than 500
shards
• Scale up to more than the shard limit for your account
### Kinesis Security
- Control access / authorization using IAM policies
- Encryption in flight using HTTPS endpoints
- Encryption at rest using KMS
- Client side encryption must be manually implemented (harder)
- VPC Endpoints available for Kinesis to access within VPC
### AWS Kinesis Data Firehose
- Fully Managed Service, no administration
- Near Real Time (60 seconds latency minimum for non full
- Load data into Redshift / Amazon S3 / ElasticSearch / Splunk
- Automatic scaling
- Supports many data formats
- Data Conversions from JSON to Parquet / ORC (only for S3)
- Data Transformation through AWS Lambda (ex: CSV => JSON)
- Supports compression when target is Amazon S3 (GZIP, ZIP, and SNAPPY)
- Only GZIP is the data is further loaded into Redshift
- Pay for the amount of data going through Firehose
- Spark / KCL do not read from KDF
### Firehose Buffer Sizing
- Firehose accumulates records in a buffer
- The buffer is flushed based on time and size rules
- Buffer Size (ex: 32MB): if that buffer size is reached, it’s flushed
- Buffer Time (ex: 2 minutes): if that time is reached, it’s flushed
- Firehose can automatically increase the buffer size to increase
throughput
- High throughput => Buffer Size will be hit
- Low throughput => Buffer Time will be hit
### Kinesis Data Streams vs Firehose
- Streams
• Going to write custom code (producer / consumer)
• Real time (~200 ms latency for classic, ~70 ms latency for enhanced fan out)
• Must manage scaling (shard splitting / merging)
• Data Storage for 1 to 7 days, replay capability, multi consumers
• Use with Lambda to insert data in real time to ElasticSearch (for example)
- Firehose
• Fully managed, send to S3, Splunk, Redshift, ElasticSearch
• Serverless data transformations with Lambda
• Near real time (lowest buffer time is 1 minute)
• Automated Scaling
• No data storage
### AWS SQS
- Oldest offering (over 10 years old)
- Fully managed
- Scales from 1 message per second to 10,000s per second
- Default retention of messages: 4 days, maximum of 14 days
- No limit to how many messages can be in the queue
- Low latency (<10 ms on publish and receive)
- Horizontal scaling in terms of number of consumers
- Can have duplicate messages (at least once delivery, occasionally)
- Can have out of order messages (best effort ordering)
- Limitation of 256KB per message sent
### SQS-Producing Messages
- Define Body
- Add message
attributes
(metadata optional)
- Provide Delay
Delivery (optional)
- Get back
- Message identifier
- MD5 hash of the body
### SQSConsuming Messages 
- Poll SQS for messages (receive up to 10 messages at a time)
- Process the message within the visibility timeout
- Delete the message using the message ID & receipt handle. No multiple consumers processed
### AWS SQS-FIFO Queue
- Newer offering (First In First out) not available in all regions!
- Name of the queue must end in . fifo
- Lower throughput (up to 3,000 per second
with batching, 300/s without)
- Messages are processed in order by the
consumer
- Messages are sent exactly once
- 5 minute interval de duplication using
“Duplication
### SQS Extended Client
- Message size limit is 256KB, how to send large messages?
- Using the SQS Extended Client (Java Library)
### AWS SQS Use Cases
- Decouple applications
(for example to handle payments
- Buffer writes to a database
(for example a voting
- Handle large loads of messages coming in
(for example an email
- SQS can be integrated with Auto Scaling through CloudWatch!
### SQS Limits
- Maximum of 120,000 in flight messages being processed by
consumers
- Batch Request has a maximum of 10 messages max 256KB
- Message content is XML, JSON, Unformatted text
- Standard queues have an unlimited TPS
- FIFO queues support up to 3,000 messages per second (using
batching)
- Max message size is 256KB (or use Extended Client)
- Data retention from 1 minute to 14 days
- Pricing:
• Pay per API Request
• Pay per network usage
### AWS SQS Security
- Encryption in flight using the HTTPS endpoint
- Can enable SSE (Server Side Encryption) using KMS
• Can set the CMK (Customer Master Key) we want to use
• SSE only encrypts the body, not the metadata
(message ID, timestamp,
- IAM policy must allow usage of SQS
- SQS queue access policy
• Finer grained control over IP
• Control over the time the requests come in
### Kinesis Data Stream vs SQS
- Kinesis Data Stream:
• Data can be consumed many times
• Data is deleted after the retention
period
• Ordering of records is preserved (at
the shard level) even during
replays
• Build multiple applications reading
from the same stream
independently (Pub/Sub)
• “Streaming MapReduce” querying
capability
• Checkpointing needed to track
progress of consumption
• Shards (capacity) must be provided ahead of time
- SQS:
• Queue, decouple applications
• One application per queue
• Records are deleted after
consumption (ack / fail)
• Messages are processed
independently for standard
queue
• Ordering for FIFO queues
• Capability to “delay” messages
• Dynamic scaling of load (no ops)
### SQS vs Kinesis
- SQS Use cases :
• Order processing
• Image Processing
• Auto scaling queues according to messages.
• Buffer and Batch messages for future processing.
• Request Offloading
- Amazon Kinesis Data Streams Use cases :
• Fast log and event data collection and processing
• Real Time metrics and reports
• Mobile data capture
• Real Time data analytics
• Gaming data feed
• Complex Stream Processing
• Data Feed from “Internet of Things”
## IoT Overview
### IoT Device Gateway

Serves as the entry point for IoT devices connecting to AWS

Allows devices to securely and efficiently communicate with AWS IoT

Supports the MQTT, WebSockets, and HTTP 1.1 protocols

Fully managed and scales automatically to support over a billion devices

No need to manage any infrastructure

### IoT Message Broker

Pub/sub (publishers/subscribers) messaging pattern - low latency

Devices can communicate with one another this way

Messages sent using the MQTT, WebSockets, or HTTP 1.1 protocols

Messages are published into topics (just like SNS)

Message Broker forwards messages to all clients connected to

the topic
### IoT Thing Registry = IAM of IoT

All connected IoT devices are represented in the AWS IoT registry

Organizes the resources associated with each device in the AWS Cloud

Each device gets a unique ID

Supports metadata for each device (ex: Celsius vs Fahrenheit, etc…)

Can create X.509 certificate to help IoT devices connect to AWS

IoT Groups: group devices together and apply permissions to the group
### Authentication


3 possible authentication methods for Things:

Create X.509 certificates and load them securely onto the Things

AWS SigV4

Custom tokens with Custom authorizers

For mobile apps:

Cognito identities (extension to Google, Facebook login, etc…)

Web / Desktop / CLI:

IAM

Federated Identities
### Authorization
### Device Shadow
### Rules Engine
### IoT Greengrass
### DMS – Database Migration Service
### DMS Sources and Targets
### AWS Schema Conversion Tool (SCT)
### Direct Connect
### Direct Connect Diagram
### Direct Connect Gateway
### Snowball
### Snowball Process
### Snowball Diagrams
### AWS Snowmobile
### MSK Managed Streaming for ApacheKafka

Fully managed Apache Kafka on AWS (alternative to Kinesis)

Allow you to create, update, delete clusters (control plane)

MSK creates & manages brokers nodes & Zookeeper nodes for you

Deploy the MSK cluster in your VPC, multi AZ (up to 3 for HA)

Automatic recovery from common Apache Kafka failures

Can create custom configurations for your clusters

Data is stored on EBS volumes

You can build producers and consumers of data (data plane)
### Apache Kafka at a high level
### MSK  Configurations
Choose the number of AZ (3 – recommended, or 2)
Choose the VPC & Subnets
The broker instance type (ex: kafka.m5.large)
The number of brokers per AZ (can add brokers later)
Size of your EBS volumes (1GB - 16 TB)
### MSK - Override Kafka Configurations
List of properties you can set: https://docs.aws.amazon.com/msk/latest/developerguide/msk-configuration-properties.html
Important to note:
Max message size in Kafka by default is 1MB
Can override this with the broker message.max.bytes setting
Must also change the consumer max.fetch.bytes setting
Latency:
By default it’s low in Kafka 10-40ms (way less than Kinesis)
The producer can increase latency to increase batching using linger.ms
### MSK Security
Can enable encryption in flight using TLS between the brokers
Can say PLAINTEXT and/or TLS-encrypted between the clients and brokers
Encryption at rest for your EBS volumes using KMS
Supports TLS client authentication using a Private Certificate Authority (CA) from ACM
Authorize specific security groups for your Apache Kafka clients
Security and ACLs for clients is done within the Apache Kafka cluster
### MSK - Monitoring    
 CloudWatch Metrics
  Basic monitoring (cluster and broker metrics)
      Enhanced monitoring (++enhanced broker metrics)
      Topic-level monitoring (++enhanced topic-level metrics)   
   Prometheus (Open-Source Monitoring)   
   Opens a port on the broker to export cluster, broker and topic-level metrics   
   Setup the JMX Exporter (metrics) or Node Exporter (CPU and disk metrics)   
   Broker Log Delivery   
   Delivery to CloudWatch Logs   
   Delivery to Amazon S3
   Delivery to Kinesis Data Firehose
### 