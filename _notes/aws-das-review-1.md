---
title: AWS DAS Collection 
layout: post
tags: [aws, das, kineisis, iot, direct, snowball]
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
- Kinesis is a managed alternative to Apache Kafka
• Great for application logs, metrics, IoT, clickstreams
• Great for “real-time” big data
• Great for streaming processing frameworks (Spark, NiFi, etc…)
- Data is automatically replicated synchronously to 3 AZ
- Kinesis Streams: low latency streaming ingest at scale
- Kinesis Analytics: perform real-time analytics on streams using
SQL
- Kinesis Firehose: load streams into S3, Redshift, ElasticSearch &
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
- Producer:
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
- Library (KPL)
- Kinesis Agent
- 3rd party libraries:Spark, Log4J Appenders, Flume,Kafka Connect, Nifi ..
### AWS Kinesis API-Exceptions
- ProvisionedThroughputExceeded Exceptions
• Happens when sending more data (exceeding MB/s or TPS for any
shard)
• Make sure you don’t have a hot shard (such as your partition key is bad
and too much data goes to that partition)
- Solution:
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
- Java based agent, built on top of KPL
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
- Serves as the entry point for IoT devices connecting to AWS
- Allows devices to securely and efficiently communicate with AWS IoT
- Supports the MQTT, WebSockets, and HTTP 1.1 protocols
- Fully managed and scales automatically to support over a billion devices
- No need to manage any infrastructure
### IoT Message Broker
- Pub/sub (publishers/subscribers) messaging pattern - low latency
- Devices can communicate with one another this way
- Messages sent using the MQTT, WebSockets, or HTTP 1.1 protocols
- Messages are published into topics (just like SNS)
- Message Broker forwards messages to all clients connected to
the topic
### IoT Thing Registry = IAM of IoT
- All connected IoT devices are represented in the AWS IoT registry
- Organizes the resources associated with each device in the AWS Cloud
- Each device gets a unique ID
- Supports metadata for each device (ex: Celsius vs Fahrenheit, etc…)
- Can create X.509 certificate to help IoT devices connect to AWS
- IoT Groups: group devices together and apply permissions to the group
### Authentication
- 3 possible authentication methods for Things:
- Create X.509 certificates and load them securely onto the Things
- AWS SigV4
- Custom tokens with Custom authorizers
- For mobile apps:
- Cognito identities (extension to Google, Facebook login, etc…)
- Web / Desktop / CLI:
- IAM
- Federated Identities
### Authorization
- AWS IoT policies:
• Attached to X.509 certificates or Cognito Identities
• Able to revoke any device at any time
• IoT Policies are JSON documents
• Can be attached to groups instead of individual Things.
- IAM Policies:
• Attached to users, group or roles
• Used for controlling IoT AWS APIs
### Device Shadow
- JSON document representing the state of a connected Thing
- We can set the state to a different desired state (ex: light on)
- The IoT thing will retrieve the state when online and adapt
### Rules Engine
- Rules are defined on the MQTT topics
- Rules = when it’s triggered | Action = what is does
- Rules use cases:
• Augment or filter data received from a device
• Write data received from a device to a DynamoDB database
• Save a file to S3
• Send a push notification to all users using SNS
• Publish data to a SQS queue
• Invoke a Lambda function to extract data
• Process messages from a large number of devices using Amazon
• Kinesis
• Send data to the Amazon Elasticsearch Service
• Capture a CloudWatch metric and Change a CloudWatch alarm
• Send the data from an MQTT message to Amazon Machine Learning to make predictions based on an Amazon ML model
& more
- Rules need IAM Roles to perform their actions
### IoT Greengrass
- IoT Greengrass brings the compute layer to the device directly
- You can execute AWS Lambda functions on the devices:
• Pre-process the data
• Execute predictions based on ML models
• Keep device data in sync
• Communicate between local devices
- Operate offline
- Deploy functions from the cloud directly to the devices
### DMS – Database Migration Service
- Quickly and securely migrate databases to AWS, resilient, self healing
- The source database remains available during the migration
- Supports:
• Homogeneous migrations: ex Oracle to Oracle
• Heterogeneous migrations: ex Microsoft SQL Server to Aurora
- Continuous Data Replication using CDC
- You must create an EC2 instance to perform the replication tasks
### DMS Sources and Targets
- SOURCES:
- On-Premise and EC2 instances databases: Oracle, MS SQL Server, MySQL, MariaDB, PostgreSQL, MongoDB, SAP, DB2
- Azure: Azure SQL Database
- Amazon RDS: all including Aurora
- Amazon S3
- TARGETS:
- On-Premise and EC2 instances databases: Oracle, MS SQL Server, MySQL, MariaDB, PostgreSQL, SAP
- Amazon RDS
- Amazon Redshift
- Amazon DynamoDB
- Amazon S3
- ElasticSearch Service
- Kinesis Data Streams
- DocumentDB
### AWS Schema Conversion Tool (SCT)
- Convert your Database’s Schema from one engine to another
- Example OLTP: (SQL Server or Oracle) to MySQL, PostgreSQL, Aurora
- Example OLAP: (Teradata or Oracle) to Amazon Redshift
- You can use AWS SCT to create AWS DMS endpoints and tasks.
### Direct Connect
- Provides a dedicated private connection from a remote network to your VPC
- Can setup multiple 1 Gbps or 10 Gbps dedicated network connections
- Setup Dedicated connection between your DC and Direct Connect locations
- You need to setup a Virtual Private Gateway on your VPC
- Access public resources (S3) and private (EC2) on same connection
- Use Cases:
- Increase bandwidth throughput - working with large data sets – lower cost
- More consistent network experience - applications using real-time data feeds
- Hybrid Environments (on prem + cloud)
- Enhanced security (private connection)
- Supports both IPv4 and IPv6
- High-availability: Two DC as failover or use Site-to-Site VPN as a failover
### Direct Connect Gateway
- If you want to setup a Direct Connect to one or more VPC in many different regions (same account), you must use a Direct Connect Gateway
### Snowball
- Physical data transport solution that helps moving TBs or PBs of data in or out of AWS
- Alternative to moving data over the network (and paying network fees)
- Secure, tamper resistant, uses KMS 256 bit encryption
- Tracking using SNS and text messages. E-ink shipping label
- Pay per data transfer job
- Use cases: large data cloud migrations, DC decommission, disaster recovery
- If it takes more than a week to transfer over the network, use Snowball devices!
### Snowball Process
- Request snowball devices from the AWS console for delivery
- Install the snowball client on your servers
- Connect the snowball to your servers and copy files using the client
- Ship back the device when you’re done (goes to the right
AWS facility)
- Data will be loaded into an S3 bucket
- Snowball is completely wiped
- Tracking is done using SNS, text messages and the AWS console
### Snowball Edge
- Snowball Edges add computational capability to the device
- 100 TB capacity with either:
- Storage optimized – 24 vCPU
- Compute optimized – 52 vCPU & optional GPU
- Supports a custom EC2 AMI so you can perform processing on the go
- Supports custom Lambda functions
• Very useful to pre-process the data while moving
• Use case: data migration, image collation, IoT Lambda capture, machine learning, AMI function
### AWS Snowmobile
- Transfer exabytes of data (1 EB = 1,000 PB = 1,000,000 TBs)
- Each Snowmobile has 100 PB of capacity (use multiple in parallel)
- Better than Snowball if you transfer more than 10 PB
### MSK Managed Streaming for ApacheKafka
- Fully managed Apache Kafka on AWS (alternative to Kinesis)
- Allow you to create, update, delete clusters (control plane)
- MSK creates & manages brokers nodes & Zookeeper nodes for you
- Deploy the MSK cluster in your VPC, multi AZ (up to 3 for HA)
- Automatic recovery from common Apache Kafka failures
- Can create custom configurations for your clusters
- Data is stored on EBS volumes
- You can build producers and consumers of data (data plane)
### Apache Kafka at a high level
### MSK  Configurations
- Choose the number of AZ (3 – recommended, or 2)
- Choose the VPC & Subnets
- The broker instance type (ex: kafka.m5.large)
- The number of brokers per AZ (can add brokers later)
- Size of your EBS volumes (1GB - 16 TB)
### MSK - Override Kafka Configurations
- List of properties you can set: https://docs.aws.amazon.com/msk/latest/developerguide/msk-configuration-properties.html
- Important to note:
- Max message size in Kafka by default is 1MB
- Can override this with the broker message.max.bytes setting
- Must also change the consumer max.fetch.bytes setting
- Latency:
- By default it’s low in Kafka 10-40ms (way less than Kinesis)
- The producer can increase latency to increase batching using linger.ms
### MSK Security
- Can enable encryption in flight using TLS between the brokers
- Can say PLAINTEXT and/or TLS-encrypted between the clients and brokers
- Encryption at rest for your EBS volumes using KMS
- Supports TLS client authentication using a Private Certificate Authority (CA) from ACM
- Authorize specific security groups for your Apache Kafka clients
- Security and ACLs for clients is done within the Apache Kafka cluster
### MSK - Monitoring    
 - CloudWatch Metrics
 - Basic monitoring (cluster and broker metrics)
 - Enhanced monitoring (++enhanced broker metrics)
 - Topic-level monitoring (++enhanced topic-level metrics)   
 - Prometheus (Open-Source Monitoring)   
 - Opens a port on the broker to export cluster, broker and topic-level metrics   
 - Setup the JMX Exporter (metrics) or Node Exporter (CPU and disk metrics)   
 - Broker Log Delivery   
 - Delivery to CloudWatch Logs   
 - Delivery to Amazon S3
 - Delivery to Kinesis Data Firehose
### Questions
- You are accumulating data from IoT devices and you must send data within 10 seconds to Amazon ElasticSearch service. That data should also be consumed by other services when needed. Which service do you recommend using?:Kinesis Data Streams
- You need a managed service that can deliver data to Amazon S3 and scale automatically for you. You want to be billed only for the actual usage of the service and be able to handle peak loads. Which service do you recommend?:Kinesis Data Firehose
- You are sending a lot of 100B data records and would like to ensure you can use Kinesis to receive your data. What should you use to ensure optimal throughput, that has asynchronous features ?:Kinesis Producer Library.Through batching (collection and aggregation), we can achieve maximum throughput using the KPL. KPL is also supporting an asynchronous API.
- You would like to collect log files in mass from your Linux servers running on premise. You need a retry mechanism embedded and monitoring through CloudWatch. Logs should end up in Kinesis. What will help you accomplish this?:Kinesis Agent
- You would like to perform batch compression before sending data to Kinesis, in order to maximize the throughput. What should you use?:Kinesis Producer Library + Implement Compression Yourself. Compression must be implemented.
- You have 10 consumers applications consuming concurrently from one shard, in classic mode by issuing GetRecords() commands. What is the average latency for consuming these records for each application?:You can issue up to 5 GetRecords API calls per second, so it'll take 2 seconds for each consuming application before they can issue their next call.Ans: 2s.
- You have 10 consumers applications consuming concurrently from one shard, in enhanced fan out mode. What is the average latency for consuming these records for each application?:70ms. here, no matter how many consumers you have, in enhanced fan out mode, each consumer will receive 2MB per second of throughput and have an average latency of 70ms.
- You would like to have data delivered in near real time to Amazon ElasticSearch, and the data delivery to be managed by AWS. What should you use?:Kinesis Firehose
- You are consuming from a Kinesis stream with 10 shards that receives on average 8 MB/s of data from various producers using the KPL. You are therefore using the KCL to consume these records, and observe through the CloudWatch metrics that the throughput is 2 MB/s, and therefore your application is lagging. What's the most likely root cause for this issue?:Your DynamoDB table is under-provisioned 
- You would like to increase the capacity of your Kinesis streams. What should you do?:Split Shards
- Which of the following statement is wrong?Spark Streaming can read from Kinesis Data Firehose
- Which of the following Kinesis Data Firehose does not write to?:DynamoDB
- You are looking to decouple jobs and ensure data is deleted after being processes. Which technology would you choose?: SQS
- You are collecting data from IoT devices at scale and would like to forward that data into Kinesis Data Firehose. How should you proceed?:Send that data into an IoT topic and define a rule action
- Which protocol is not supported by the IoT Device Gateway?:FTP
- You would like to control the target temperature of your room using an IoT thing thermostat. How can you change its state for target temperature even in the case it's temporarily offline?Change the state of the device shadow.That's precisely the purposes of the device shadow, which gets synchronized with the device when it comes back online
- You are looking to continuously replicate a MySQL database that's on premise to Aurora. Which service will allow you to do so securely?Database Migration Services.DMS is fully secure
- You have setup Direct Connect on one location to ensure your traffic into AWS is going over a private network. You would like to setup a failover connection, that must be as reliable and as redundant as possible, as you cannot afford to be down for too long. What backup connection do you recommend?:Direct Setup is although this is more secure than Site to Site VPN, it is less reliable as it does not leverage the public web. It is the wrong answer here, but a correct answer overall for setting up highly available Direct Connect. Site to site VPN is although this is not as private as another Direct Connect setup, it is definitely more reliable as it leverages the public web. It is the correct answer here.
- You would like to transfer data in AWS in less than two days from now. What should you use?:Setting up direct link takes a long time, or snowball. Use public internet.
