---
title: AWS SAA Training notes, day 1
layout: post
tags: [aws, saa, test, training]
date: 2021-05-19
---

- Optimized EBS: spearate IO  for ebs and other traffic
- no snapshot for ephemeral storages
- EBS is only attaches to one instance
- S3 is option
- EFS is scalable , shared. Across zones, regions, vpc account. EFS is just for linux.

## EC2 instance types
- t is burstable
- m is memory
- generation member is number after. (exp. m5)
- general usage customers: usually use  t3 unlimted . Collecting credits during lower usage and use credits later.
### Compute optimized
- type c. Compute intensive type
- Elastic Fabric Adapter, or EFA, is a network adapter for Amazon EC2 instances that delivers the performance of on-premises HPC clusters with the elasticity and scalability of AWS. You can run HPC applications that require high levels of inter-instance communications, such as computational fluid dynamics, weather modeling, and reservoir simulation. In addition, HPC applications use popular HPC technologies, such as Message Passing Interface (MPI), which can scale to thousands of CPU cores. EFA supports industry-standard libfabric APIs, so applications that use a supported MPI library can be migrated to AWS with little or no modificati
#### Memory  optimized
- type r . Need more RAM
### Accelarated Computing Example
- Type p model. Machine deep learning.
### Storage optimized
- type H model. High disk thgroughput.HDFS.

### Different processors  have different properties, so they can affect performences.
## Instance generation and cost
- Newer genereatio has better price to performance 
## Pricing type
Amazon EC2 usage of Amazon Linux-and Ubuntu-based instances that are launched in On-Demand, Reserved and Spot form will be billed on one-second increments, with a minimum of 60 seconds. All other operating systems are billed in one-hour increments, and are billed hour forward, that is, billed at the start of the hour whether you use the full hour or not. Note that Reserved Instances are launched as, and indistinguishable from, On-Demand Instances until the bill is processed. Compute Savings Plans provide the most flexibility and help to reduce your costs by up to 66% (just like Convertible RIs). The plans automatically apply to any EC2 instance regardless of Region, instance family, operating system, or tenancy, including those that are part of EMR, ECS, or EKS clusters, or launched by Fargate. For example, you can shift from C4 to C5 instances, move a workload from Dublin to London, or migrate from EC2 to Fargate, benefiting from Savings Plan prices along the way, without having to do anything.
### On-Demand
### Reserved
#### Standard, Convertible, Scheduled.
#### Paying upfront payments. Pre-pay.Shared.
### Saving plan
#### Most flexibility  
### Spot 
 
### Dedicated instances
 - Dedicated Instances are Amazon EC2 instances that run in a VPC on hardware that's dedicated to a single customer. Your Dedicated Instances are physically isolated at the host hardware level from instances that belong to other AWS accounts. Dedicated Instance pricing has two components:
### Dedicated hosts
- A Dedicated Host is a physical EC2 server with instance capacity fully dedicated for your use. Dedicated Hosts can help you reduce costs by allowing you to use your existing server-bound software licenses, 
#### limitations
- After you launch an instance, there are some limitations to changing its tenancy. • You cannot change the tenancy of an instance from default to dedicated or host after you've launched it.
• You cannot change the tenancy of an instance from dedicated or host to default after you've launched it.
• You can change the tenancy of an instance from dedicated to host, or from host to dedicated, after you've launched it.
 
### AWS Compute optimizer: Cost optimizator
### Tags in AWS
- Netflix script 
- https://d1.awsstatic.com/whitepapers/aws-tagging-best-practices.pdf
### AWS resources
- The cluster placement group is a logical grouping of instances within a single Availability Zone. This grouping provides the lowest latency and highest packet per second network performance possible.
- A spread placement group is a grouping of instances that are purposely positioned on distinct underlying hardware. This grouping reduces the risk of simultaneous failures that could occur if instances were sharing underlying hardware
- Partition placement groups spread EC2 instances across logical partitions and ensure that instances in different partitions do not share the same underlying hardware, thus containing the impact of hardware failure to a single partition.


## Database layer