---
title: AWS DAS Service integration 
layout: post
tags: [aws, das, ec2, integration]
date: 2020-10-01
---
### AWS Services Integration
- IoT topic, IoT rules, IoT destinations ->kinesis,dynamodb,sqs,s3,lambda...
- Kinesis data streams:
- Producers: SDK, KPL, Kinesis agent, Spark, Kafka Connect
- Consumers: Spark, Firehouse, Lambda, KCL, SDK, Kinesis connect library
- Firehouse to deliver data, lambda for transformation:
- Producers:KPL, Kinesis Agent, CloudWatch logs, IoT rules, Kinesis DataStreams
- Consumers: S3, RedShift, Elastic, Splunk
- Kinesis Data Analytics, transform in lambda
- Producers: Streams, Firehouse, Reference data(csv) in S3
- Consumerts: Kinesis data streams, Firehouse, Lambda
- SQS
- Producers: SDK, IoT, S3
- Consumers: SDK, Lambda
- S3
- Producers: a lot 
- Consumers: SQS, SNS, Lambda
- DynamoDB, transform AWS pipeline
- Producers: SDK,DMS
- Consumers: Glue, EMR(hive), DynamoDB streams, later Lambda, KCL
- Glue
- Producers:DynamoDB, S3, JDBC
- Consumers:RedShift,Athena,EMR+HIVE
- EMR is Hadoop,Hive,Spark,Presto, Jupyter, Flink
- Producers: DynamoDB, Apache Ranger, S3/EMRFS, GLUE
- Machine Learning is deprecated
- Producers: S3, Readshift
- Amazon SageMaker- fancy machine learning
- Producers:S3
- Consumers:results in notebook
- AWS data pipeline
- integrate S3, EMR, JDBC, DynamoDB
- Elastic
- Producers:Firehouse,IoT core, CloudWatch logs
- Athena:
- Producers:S3, Glue 
- Consumers:S3,QuickSight
- RedShift:
- Producers:S3
- Consumers:QuickSight, PSQL-dblink
- QuickSight
- Producers: RedShift, Aurora, JDBC, Athena, S3
### AWS Instance Types
General Purpose: T2, T3, M4, M5
Compute Optimized: C4, C5
Batch processing, Distributed analytics, Machine / Deep Learning Inference
Memory Optimized: R4, R5, X1, Z1d
High performance database, In memory database, Real time big data analytics
Accelerated Computing: P2, P3, G3, F1
GPU instances, Machine or Deep Learning, High Performance Computing
Storage Optimized: H1, I3, D2
Distributed File System (HDFS), NFS, Map Reduce, Apache Kafka, Redshift
### EC2 in Big Data
On demand, Spot & Reserved instances:
Spot: can tolerate loss, low cost => checkpointing feature (ML, etc)
Reserved: long running clusters, databases (over a year)
On demand: remaining workloads
Auto Scaling:
Leverage for EMR, etc
Automated for DynamoDB, Auto Scaling Groups, etc…
EC2 is behind EMR
Master Nodes
Compute Nodes (contain data) + Tasks Nodes (do not contain data)