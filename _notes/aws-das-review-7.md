---
title: AWS DAS Service integration 
layout: post
tags: [aws, das, ec2, integration]
date: 2020-10-01
---
###
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