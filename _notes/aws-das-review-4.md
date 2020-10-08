---
title: AWS DAS Processing 
layout: post
tags: [aws, das, glue, lambda, hive, spark, flume]
date: 2020-10-01
---

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
- EBS for HDFS
###
###