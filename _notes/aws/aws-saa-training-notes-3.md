---
title: AWS SAA Training notes, day 3
layout: post
tags: [aws, saa, test, training]
date: 2021-05-21
---

### Availability
- good idea: several bids in different availability zones
#### Autoscaling
- Autohealing: min 1 max 1 desired 1
- Thrashing: mess in scaling , up and downs.
- Amazon linux + Ubuntu are charged per seconds. (No license)
- EC2+RH its payed by hour. Every time one hour. Using resources on different host, license.
#### Scaling DB
- Scale of read-heavy, offload reporting. Replication is asynchronous.
- Another way of scaling is sharding. Sharding is like partition. Have to create a sharding instance.
- Push the button
### Automation
### Caching
### Availability
### Decoupled Acrhitectures
### Microservices
### RTO/RPO and Backup Recovery
- 4 Common Practices for Disaster Recovery
- Backup and Restore:Lower priority user case. Lower cost of solution.
- Pilog light: Meeting lower RTO and RPO requirements. Core Services, scales in response of DR event. RPO/RTO=10min
- Fully working Low-Capacity Standby. Solution that require RTO and RPO in min. BUsiness-critical services. RPO/RTO: Minutes. 
- Multi-Site Active-Active: Auto-failover of your environment in AWS to a running duplicate. RPO/RTO Real-time. Most expensive.
### Optimization
- Leverage Different Pricing Models in Amazon EC2. First floor: Reserved Instances, than a fill of On-Demand and then Spot instance. 