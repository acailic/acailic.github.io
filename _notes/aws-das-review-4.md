---
title: AWS DAS Processing 
layout: post
tags: [aws, das, glue, lambda, hive, spark, flume]
date: 2020-10-01
---
## Lambda
## Glue
## EMR-Elastic MapReduce
### EMR
- Elastic MapReduce 
- Managed Hadoop Cluster on EC2 instances
- includes Spark,Hive,HBase,Presto, Flink and more
- EMR Notebooks-query data; several integrations with AWS. Similar to Cloudera.
#### EMR Cluster
- Master node- manages the cluster, single EC2 instance
- Core node-hosts HDFS data and runs task. Can be scaled with risks. at least one is present
- Task node- runs tasks and does not stores data. good for spot instance. no risk of losing data on remove.
#### EMR Usage
- Transient vs Long-Running cluster: Transient its shuts down after finishing tasks.
• spin task nodes temporary using spot instances 
• using reserved instances for long running to save money
- Connect directly to master to run jobs.
- Submit order via the console
#### EMR AWS integration
- uses EC2 nodes to in clusters
- using VPC to configure the virtual network
- S3 to store input and output data
- CloudWatch to monitor cluster performance and configure alarms
- IAM to configure permissions
- CloudTrail to audit requests made to the service
- Data Pipeline to schedule and start your clusters
#### EMR Storage
- HDFS- distributed file system. 128mb block.
- EMRFS: access S3 as if it were HDFS
• EMRFS Consistent View – Optional for S3 consistency
• Uses DynamoDB to track consistency
- Local file system
- EBS for HDFS. you can reduce storage if not needed for reducing costs.
#### EMR promises
- EMR charges by the hour
• Plus EC2 charges
- Provisions new nodes if a core node fails
- Can add and remove task on the fly
- can resize running cluster's core nodes
#### Hadoop
- Architecture-MapReduce, YARN, HDFS
- if it turns down cluste, data goes bye bye
-  YARN: yet another resource negotiatior
-  MapReduce algorithm
### Spark
- Architecture-MapReduce & Spark, YARN, HDFS. a lot of stuff in memory.
- Spark Streaming, analytics, ML, Hive, sql.
- How it works:
- Driver Program: Spark context, code you write.
- Cluster Manager: Spark, YARN
- Executors: each has cache and code task
##### Spark Components
- Spark Core on bottom. RDD 
- Up: streaming, sql, graphX, MLLib
- Spark SQL - distributed query language, low latency, optimized, hive,sql. like a giant DB.
- Streaming- real time solution to stream analytics. ingest data in mini batches. same code for real time and batch processing.
- MLLib - library for machine learning 
- GraphX- distributed graph, graph in db sense, like social network nodes. ETL
#### Spark structured streaming 
- Streaming as a db that grows, virtual db set. it can be queried on moments in time. it resolves reliability.
- Spark streaming can be integrated with kinesis. Spark kinesis package
- It can be connected to redshift. redshift kinesis package. uses it like a source. Useful for ETL using Spark. 
#### Hive on EMR
- Architecture: Hadoop YARN, MapReduce & Tez, Hive
- Tez is like spark. MapReduce is fallen into past from new technologies.
- HiveQL- familar to SQL, Interactive.
- Scalable - works with big data on a cluster. Most appropriate for data warehouse apps
- Easy OLAP queries - easier than MapReduce
- Highly optimized, extensible
- Hive maintains a metastore that imparts a structure you define on unstructured data that is stored on HDFS. Metastore is stored in MySql on the master node by default. External metastores offer better integration like: Glue, RDS.
- it can load table partitions from S3, write tables in s3,load scipts from S3, has dynamodb as external table
#### Pig on EMR
- Alternative interface from MapReduce. PigLatin language. Older.
- Writing mappers and reducers by hand takes a long time.
- Pig introduces Pig Latin, a scripting language that lets you use SQL-like syntax to define your map and reduce steps.
- Highly extensible with user-defined functions (UDF’s)
- Architecture: Hadoop YARN, MapReduce & Tez, Pig
#### HBase on EMR
- Non-relational, petabyte-scale database
- Based on Google’s BigTable, on top of HDFS
- In-memory
- Hive integration
- Sounds a lot like DynamoDB
- Both are NoSQL databases intended for the same sorts of things
- But if you’re all-in with AWS anyhow, DynamoDB has advantages
• Fully managed (auto-scaling)
• More integration with other AWS services
• Glue integration
- HBase has some advantages though:
• Efficient storage of sparse data
• Appropriate for high frequency counters (consistent reads & writes)
• High write & update throughput
• More integration with Hadoop
#### Presto on EMR
It can connect to many different “big data” databases and data stores at once, and query across them
Interactive queries at petabyte scale
Familiar SQL syntax
Optimized for OLAP – analytical queries, data warehousing
Developed, and still partially maintained by Facebook
This is what Amazon Athena uses under the hood
Exposes JDBC, Command-Line, and Tableau interfaces
### Apache Zeppelin