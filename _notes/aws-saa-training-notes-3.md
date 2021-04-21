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
- Sclae of read-heavy, offload reporting. Replication is asynchronous.
- Another way of scaling is sharding. Sharding is like partition. Have to create a sharding instance.
- Push the button
### Automation
### Caching
### Availability
### Decoupled Acrhitectures
### Microservices
### RTO/RPO and Backup Recovery
### Optimization
### Wrap up 