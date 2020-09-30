---
title: AWS Fundamentals  IAM, EC2
layout: post
tags: [aws, cda, iam, ec2]
date: 2020-09-17
---

### AWS Fundamentals: IAM, EC2
### AWS Regions
• AWS has Regions all around the world
• Names can be: us-east-1, eu-west-3…
• A region is a cluster of data centers
• Most AWS services are region-scoped
### AWS Availability Zones
• Each region has many availability zones
(usually 3, min is 2, max is 6). Example:
• ap-southeast-2a
• ap-southeast-2b
• ap-southeast-2c
• Each availability zone (AZ) is one or more
discrete data centers with redundant power,
networking, and connectivity
• They’re separate from each other, so that
they’re isolated from disasters
• They’re connected with high bandwidth,
ultra-low latency networking
### IAM Introduction
• IAM (Identity and Access Management)
• Your whole AWS security is there:
• Users
• Groups
• Roles
• Root account should never be used (and shared)
• Users must be created with proper permissions
• IAM is at the center of AWS
• Policies are written in JSON (JavaScript Object Notation)
### IAM Federation
• Big enterprises usually integrate their own repository of users with IAM
• This way, one can login into AWS using their company credentials
• Identity Federation uses the SAML standard (Active Directory)
### IAM 101 Brain Dump
• One IAM User per PHYSICAL PERSON
• One IAM Role per Application
• IAM credentials should NEVER BE SHARED
• Never, ever, ever, ever, write IAM credentials in code. EVER.
• And even less, NEVER EVER EVER COMMIT YOUR IAM credentials
• Never use the ROOT account except for initial setup.
• Never use ROOT IAM Credentials
### What is EC2?
• EC2 is one of most popular of AWS offering
• It mainly consists in the capability of :
• Renting virtual machines (EC2)
• Storing data on virtual drives (EBS)
• Distributing load across machines (ELB)
• Scaling the services using an auto-scaling group (ASG)
• Knowing EC2 is fundamental to understand how the Cloud works
### Hands-On:Launching an EC2 Instance running Linux
• We’ll be launching our first virtual server using the AWS Console
• We’ll get a first high level approach to the various parameters
• We’ll learn how to start / stop / terminate our instance.
### EC2 Instance Connect
• Connect to your EC2 instance within your browser
• No need to use your key file that was downloaded
• The “magic” is that a temporary key is uploaded onto EC2 by AWS
• Works only out-of-the-box with Amazon Linux 2
• Need to make sure the port 22 is still opened!
### Introduction to Security Groups
• Security Groups are the fundamental of network security in AWS
• They control how traffic is allowed into or out of our EC2 Machines.
• It is the most fundamental skill to learn to troubleshoot networking
issues
• In this lecture, we’ll learn how to use them to allow, inbound and
outbound ports
### Security Groups Good to know
• Can be attached to multiple instances
• Locked down to a region / VPC combination
• Does live “outside” the EC2 – if traffic is blocked the EC2 instance won’t see
it
• It’s good to maintain one separate security group for SSH access
• If your application is not accessible (time out), then it’s a security group issue
• If your application gives a “connection refused“ error, then it’s an application
error or it’s not launched
• All inbound traffic is blocked by default
• All outbound traffic is authorised by default
### Private vs Public IP (IPv4)
• Networking has two sorts of IPs. IPv4 and IPv6:
• IPv4: 1.160.10.240
• IPv6: 3ffe:1900:4545:3:200:f8ff:fe21:67cf
• In this course, we will only be using IPv4.
• IPv4 is still the most common format used online.
• IPv6 is newer and solves problems for the Internet of Things (IoT).
• IPv4 allows for 3.7 billion different addresses in the public space
• IPv4: [0-255].[0-255].[0-255].[0-255].
#### Private vs Public IP (IPv4)
Fundamental Differences
• Public IP:
• Public IP means the machine can be identified on the internet (WWW)
• Must be unique across the whole web (not two machines can have the same public IP).
• Can be geo-located easily
• Private IP:
• Private IP means the machine can only be identified on a private network only
• The IP must be unique across the private network
• BUT two different private networks (two companies) can have the same IPs.
• Machines connect to WWW using a NAT + internet gateway (a proxy)
• Only a specified range of IPs can be used as private IP
#### Elastic IPs
• When you stop and then start an EC2 instance, it can change its public
IP.
• If you need to have a fixed public IP for your instance, you need an
Elastic IP
• An Elastic IP is a public IPv4 IP you own as long as you don’t delete it
• You can attach it to one instance at a time
#### Elastic IP
• With an Elastic IP address, you can mask the failure of an instance or software
by rapidly remapping the address to another instance in your account.
• You can only have 5 Elastic IP in your account (you can ask AWS to increase
that).
• Overall, try to avoid using Elastic IP:
• They often reflect poor architectural decisions
• Instead, use a random public IP and register a DNS name to it
• Or, as we’ll see later, use a Load Balancer and don’t use a public IP
#### EC2 User Data
• It is possible to bootstrap our instances using an EC2 User data script.
• bootstrapping means launching commands when a machine starts
• That script is only run once at the instance first start
• EC2 user data is used to automate boot tasks such as:
• Installing updates
• Installing software
• Downloading common files from the internet
• Anything you can think of
• The EC2 User Data Script runs with the root user
#### EC2 Instance Launch Types
• On Demand Instances: short workload, predictable pricing
• Reserved: (MINIMUM 1 year)
• Reserved Instances: long workloads
• Convertible Reserved Instances: long workloads with flexible instances
• Scheduled Reserved Instances: example – every Thursday between 3 and 6 pm
• Spot Instances: short workloads, for cheap, can lose instances (less reliable)
• Dedicated Instances: no other customers will share your hardware
• Dedicated Hosts: book an entire physical server, control instance placement
#### EC2 On Demand
• Pay for what you use (billing per second, after the first minute)
• Has the highest cost but no upfront payment
• No long term commitment
• Recommended for short-term and un-interrupted workloads, where
you can't predict how the application will behave.
#### EC2 Reserved Instances
• Up to 75% discount compared to On-demand
• Pay upfront for what you use with long term commitment
• Reservation period can be 1 or 3 years
• Reserve a specific instance type
• Recommended for steady state usage applications (think database)
• Convertible Reserved Instance
• can change the EC2 instance type
• Up to 54% discount
• Scheduled Reserved Instances
• launch within time window you reserve
• When you require a fraction of day / week / month
#### EC2 Spot Instances
• Can get a discount of up to 90% compared to On-demand
• Instances that you can “lose” at any point of time if your max price is less than the
current spot price
• The MOST cost-efficient instances in AWS
• Useful for workloads that are resilient to failure
• Batch jobs
• Data analysis
• Image processing
• …
• Not great for critical jobs or databases
• Great combo: Reserved Instances for baseline + On-Demand & Spot for peaks
#### EC2 Dedicated Hosts
• Physical dedicated EC2 server for your use
• Full control of EC2 Instance placement
• Visibility into the underlying sockets / physical cores of the hardware
• Allocated for your account for a 3 year period reservation
• More expensive
• Useful for software that have complicated licensing model (BYOL –
Bring Your Own License)
• Or for companies that have strong regulatory or compliance needs
#### EC2 Dedicated Instances
• Instances running on
hardware that’s dedicated to
you
• May share hardware with
other instances in same
account
• No control over instance
placement (can move
hardware after Stop / Start)
#### Which host is right for me?
• On demand: coming and staying in resort
whenever we like, we pay the full price
• Reserved: like planning ahead and if we plan to
stay for a long time, we may get a good
discount.
• Spot instances: the hotel allows people to bid
for the empty rooms and the highest bidder
keeps the rooms. You can get kicked out at any
time
• Dedicated Hosts: We book an entire building
of the resort
#### EC2 Pricing
• EC2 instances prices (per hour) varies based on these parameters:
• Region you’re in
• Instance Type you’re using
• On-Demand vs Spot vs Reserved vs Dedicated Host
• Linux vs Windows vs Private OS (RHEL, SLES, Windows SQL)
• You are billed by the second, with a minimum of 60 seconds.
• You also pay for other factors such as storage, data transfer, fixed IP
public addresses, load balancing
• You do not pay for the instance if the instance is stopped
#### What’s an AMI?
• As we saw, AWS comes with base images such as:
• Ubuntu
• Fedora
• RedHat
• Windows
• Etc…
• These images can be customised at runtime using EC2 User data
• But what if we could create our own image, ready to go?
• That’s an AMI – an image to use to create our instances
• AMIs can be built for Linux or Windows machines
#### Why would you use a custom AMI?
• Using a custom built AMI can provide the following advantages:
• Pre-installed packages needed
• Faster boot time (no need for long ec2 user data at boot time)
• Machine comes configured with monitoring / enterprise software
• Security concerns – control over the machines in the network
• Control of maintenance and updates of AMIs over time
• Active Directory Integration out of the box
• Installing your app ahead of time (for faster deploys when auto-scaling)
• Using someone else’s AMI that is optimised for running an app, DB, etc…
• AMI are built for a specific AWS region (!)
#### EC2 Instances Overview
• Instances have 5 distinct characteristics advertised on the website:
• The RAM (type, amount, generation)
• The CPU (type, make, frequency, generation, number of cores)
• The I/O (disk performance, EBS optimisations)
• The Network (network bandwidth, network latency)
• The Graphical Processing Unit (GPU)
• It may be daunting to choose the right instance type (there are over 50 of them) -
https://aws.amazon.com/ec2/instance-types/
• https://ec2instances.info/ can help with summarizing the types of instances
• R/C/P/G/H/X/I/F/Z/CR are specialised in RAM, CPU, I/O, Network, GPU
• M instance types are balanced
• T2/T3 instance types are “burstable”
#### Burstable Instances (T2)
• AWS has the concept of burstable instances (T2 machines)
• Burst means that overall, the instance has OK CPU performance.
• When the machine needs to process something unexpected (a spike in
load for example), it can burst, and CPU can be VERY good.
• If the machine bursts, it utilizes “burst credits”
• If all the credits are gone, the CPU becomes BAD
• If the machine stops bursting, credits are accumulated over time
• Burstable instances can be amazing to handle unexpected traffic and
getting the insurance that it will be handled correctly
• If your instance consistently runs low on credit, you need to move to a
different kind of non-burstable instance (all the ones described before).
#### Elastic Network Interfaces (ENI)
• Logical component in a VPC that represents a
virtual network card
• The ENI can have the following attributes:
• Primary private IPv4, one or more secondary IPv4
• One Elastic IP (IPv4) per private IPv4
• One Public IPv4
• One or more security groups
• A MAC address
• You can create ENI independently and attach
them on the fly (move them) on EC2 instances
for failover
• Bound to a specific availability zone (AZ)
#### Questions
-
