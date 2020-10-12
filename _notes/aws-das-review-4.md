---
title: AWS DAS Processing 
layout: post
tags: [aws, das, glue, lambda, hive, spark, flume]
date: 2020-10-01
---
## Lambda
### Questions
- You are going to be working with objects arriving in S3. Once they arrive you want to use AWS Lambda as a part of an AWS Data Pipeline to process and transform the data. 
How can you easily configure Lambda to know the data has arrived in a bucket?:Configure S3 bucket notifications to lambda.Lambda functions are generally invoked by some sort of trigger. S3 has the ability to trigger a Lambda function whenever a new object appears in a bucket.
- You are going to analyze the data coming in an Amazon Kinesis stream. You are going to use Lambda to process these records. What is a prerequisite when it comes to defining Lambda to access Kinesis stream records ?: Lambda must be in the same account as the service triggering it, in addition to having an IAM policy granting it access.
- How can you make sure your Lambda functions have access to the other resources you are using in your big data architecture like S3, Redshift, etc.?:Using proper IAM roles.IAM roles define the access a Lambda function has to the services it communicates with.
- You are creating a Lambda - Kinesis stream environment in which Lambda is to check for the records in the stream and do some processing in its Lambda function. 
How does Lambda know there has been changes / updates to the Kinesis stream ? Lambda polls Kinesis streams. Although you think of a trigger as "pushing" events, Lambda actually polls your Kinesis streams for new activity.
- When using an Amazon Redshift database loader, how does Lambda keep track of files arriving in S3 to be processed and sent to Redshift ?:In a DynamoDB table
## Glue
### Questions
- You want to load data from a MySQL server installed in an EC2 t2.micro instance to be processed by AWS Glue. What applies the best here?:Instance should be in your VPC.Although we didn't really discuss access controls, you could arrive at this answer through process of elimination. You'll find yourself doing that on the exam a lot. This isn't really a Glue specific question; it's more about how to connect an AWS service such as Glue to EC2.
- What is the simplest way to make sure the metadata under Glue Data Catalog is always up-to-date and in-sync with the underlying data without your intervention each time?:Crawlers may be easily scheduled to run periodically while defining them.
- Which programming languages can be used to write ETL code for AWS Glue?:Python and Scala.Glue ETL runs on Apache Spark under the hood, and these happen to be the primary languages used for Spark development.
- Can you run existing ETL jobs with AWS Glue?:Yes. You can run your existing Scala or Python code on AWS Glue. Simply upload the code to Amazon S3 and create one or more jobs that use that code. You can reuse the same code across multiple jobs by pointing them to the same code location on Amazon S3.
- How can you be notified of the execution of AWS Glue jobs?:AWS Glue outputs its progress into CloudWatch, which in turn may be integrated with the Simple Notification Service.
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
- It can connect to many different “big data” databases and data stores at once, and query across them
- Interactive queries at petabyte scale
- Familiar SQL syntax
- Optimized for OLAP – analytical queries, data warehousing
- Developed, and still partially maintained by Facebook
- This is what Amazon Athena uses under the hood
- Exposes JDBC, Command-Line, and Tableau interfaces
- Connectors: HDFS, S3, Cassandra, MongoDB, HBase, SQL, RedShift, Teradata
### Apache Zeppelin
- If you’re familiar with iPython notebooks – it’s like that
• Lets you interactively run scripts / code against your data
• Can interleave with nicely formatted notes
• Can share notebooks with others on your cluster
- Spark, Python, JDBC, HBase, Elasticsearch + more
#### Zeppelin + Spark
- Can run Spark code interactively (like you can in the Spark shell)
• This speeds up your development cycle
• And allows easy experimentation and exploration of your big data
- Can execute SQL queries directly against SparkSQL
- Query results may be visualized in charts and graphs
- Makes Spark feel more like a data science tool!
#### EMR Notebook
- Similar concept to Zeppelin, with more AWS integration
- Notebooks backed up to S3
- Provision clusters from the notebook!
- Hosted inside a VPC
- Accessed only via AWS console
#### Hue
- Hadoop User Experience
- Graphical front-end for applications on your EMR cluster
- IAM integration: Hue Super-users inherit IAM roles
- S3: Can browse & move data between HDFS and S3
#### Splunk
- Splunk / Hunk “makes machine data accessible, usable, and valuable to everyone”
- Operational tool – can be used to visualize EMR and S3 data using your EMR Hadoop cluster.
#### Flume
- Another way to stream data into your cluster
- Made from the start with Hadoop in mind
- Built-in sinks for HDFS and HBase
- Originally made to handle log aggregation
#### MXNet
- Like Tensorflow, a library for building and accelerating neural networks
- Included on EMR
#### S3DistCP
- Tool for copying large amounts of data
• From S3 into HDFS
• From HDFS into S3
- Uses MapReduce to copy in a distributed manner
- Suitable for parallel copying of large numbers of objects
• Across buckets, across accounts
#### Other EMR / Hadoop Tools
- Ganglia (monitoring)
- Mahout (machine learning)
- Accumulo (another NoSQL database)
- Sqoop (relational database connector)
- HCatalog (table and storage management for Hive metastore)
- Kinesis Connector (directly access Kinesis streams in your scripts)
- Tachyon (accelerator for Spark)
- Derby (open-source relational DB in Java)
- Ranger (data security manager for Hadoop)
- Install whatever you want
### EMR security
- IAM policies: services,iam with taging per cluster, accesing files on cluster
- Kerberos: authentication, cryptography. 5.10 and later versions
- SSH: tunneling, key-pairs
- IAM roles:  service role and EC2 instance profile
#### EMR: Choosing Instance Types
- Master node:
• m4.large if < 50 nodes, m4.xlarge if > 50 nodes
- Core & task nodes:
• m4.large is usually good
• If cluster waits a lot on external dependencies (i.e. a web crawler), t2.medium
• Improved performance: m4.xlarge
• Computation-intensive applications: high CPU instances
• Database, memory-caching applications: high memory instances
• Network / CPU-intensive (NLP, ML) – cluster computer instances
- Spot instances
• Good choice for task nodes
• Only use on core & master if you’re testing or very cost-sensitive; you’re risking partial data loss
### Questions
- Which one of the following statements is NOT TRUE regarding EMR Notebooks?
- 
- 
- 
- 
### AWS Data Pipeline
- Let you schedule tasks to organize. Like weekly. Move data between services.
- Destinations include S3, RDS, DynamoDB, Redshift and EMR
- Manages task dependencies
- Retries and notifies on failures
- Cross-region pipelines
- Precondition checks
- Data sources may be on-premises
- Highly available
- Activates: EMR,Hive,Copy,SQL,Scripts
### AWS Step Functions
- Use to design workflows
- Easy visualizations
- Advanced Error Handling and Retry mechanism outside the code
- Audit of the history of workflows
- Ability to “Wait” for an arbitrary amount of time
- Max execution time of a State Machine is 1 year
- Step Functions – Examples Train a Machine Learning Model