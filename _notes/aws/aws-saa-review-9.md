---
title: AWS SAA Practise test 9
layout: post
tags: [aws, saa, test]
date: 2021-04-18
--- 
-
- Elastic Fabric Adapter
  
  An Elastic Fabric Adapter (EFA) is a network device that you can attach to your Amazon EC2 instance to accelerate High Performance Computing (HPC) and machine learning applications. It enhances the performance of inter-instance communication that is critical for scaling HPC and machine learning applications. EFA devices provide all Elastic Network Adapter (ENA) devices functionalities plus a new OS bypass hardware interface that allows user-space applications to communicate directly with the hardware-provided reliable transport functionality.
- Create an inbound endpoint on Route 53 Resolver and then DNS resolvers on the on-premises network can forward DNS queries to Route 53 Resolver via this endpoint
  
  Create an outbound endpoint on Route 53 Resolver and then Route 53 Resolver can conditionally forward queries to resolvers on the on-premises network via this endpoint
  
  Amazon Route 53 is a highly available and scalable cloud Domain Name System (DNS) web service. Amazon Route 53 effectively connects user requests to infrastructure running in AWS – such as Amazon EC2 instances – and can also be used to route users to infrastructure outside of AWS. By default, Route 53 Resolver automatically answers DNS queries for local VPC domain names for EC2 instances. You can integrate DNS resolution between Resolver and DNS resolvers on your on-premises network by configuring forwarding rules.
  
  To resolve any DNS queries for resources in the AWS VPC from the on-premises network, you can create an inbound endpoint on Route 53 Resolver and then DNS resolvers on the on-premises network can forward DNS queries to Route 53 Resolver via this endpoint.
- S3 can encrypt object metadata by using Server-Side Encryption
  
  Amazon S3 is a simple key-value store designed to store as many objects as you want. You store these objects in one or more buckets, and each object can be up to 5 TB in size.
  
  An object consists of the following:
  
  Key – The name that you assign to an object. You use the object key to retrieve the object.
  
  Version ID – Within a bucket, a key and version ID uniquely identify an object.
  
  Value – The content that you are storing.
  
  Metadata – A set of name-value pairs with which you can store information regarding the object.
  
  Subresources – Amazon S3 uses the subresource mechanism to store object-specific additional information.
  
  Access Control Information – You can control access to the objects you store in Amazon S3.
  
  Metadata, which can be included with the object, is not encrypted while being stored on Amazon S3. Therefore, AWS recommends that customers not place sensitive information in Amazon S3 metadata.
- Use AWS Database Migration Service to replicate the data from the databases into Amazon Redshift
  
  AWS Database Migration Service helps you migrate databases to AWS quickly and securely. The source database remains fully operational during the migration, minimizing downtime to applications that rely on the database. With AWS Database Migration Service, you can continuously replicate your data with high availability and consolidate databases into a petabyte-scale data warehouse by streaming data to Amazon Redshift and Amazon S3.
- NAT instance can be used as a bastion server
  
  Security Groups can be associated with a NAT instance
  
  NAT instance supports port forwarding
  
  A NAT instance or a NAT Gateway can be used in a public subnet in your VPC to enable instances in the private subnet to initiate outbound IPv4 traffic to the Internet.
- Use WAF geo match statement listing the countries that you want to block
  
  Use WAF IP set statement that specifies the IP addresses that you want to allow through
  
  AWS WAF is a web application firewall that helps protect your web applications or APIs against common web exploits that may affect availability, compromise security, or consume excessive resources. AWS WAF gives you control over how traffic reaches your applications by enabling you to create security rules that block common attack patterns and rules that filter out specific traffic patterns you define.
  
  You can deploy AWS WAF on Amazon CloudFront as part of your CDN solution, the Application Load Balancer that fronts your web servers or origin servers running on EC2, or Amazon API Gateway for your APIs.
- Delete the existing standard queue and recreate it as a FIFO queue
  
  Make sure that the name of the FIFO queue ends with the .fifo suffix
  
  Make sure that the throughput for the target FIFO queue does not exceed 3,000 messages per second
  
  Amazon Simple Queue Service (SQS) is a fully managed message queuing service that enables you to decouple and scale microservices, distributed systems, and serverless applications. SQS eliminates the complexity and overhead associated with managing and operating message oriented middleware, and empowers developers to focus on differentiating work. Using SQS, you can send, store, and receive messages between software components at any volume, without losing messages or requiring other services to be available.
  
  SQS offers two types of message queues. Standard queues offer maximum throughput, best-effort ordering, and at-least-once delivery. SQS FIFO queues are designed to guarantee that messages are processed exactly once, in the exact order that they are sent.
  
  By default, FIFO queues support up to 3,000 messages per second with batching, or up to 300 messages per second (300 send, receive, or delete operations per second) without batching. Therefore, using batching you can meet a throughput requirement of upto 3,000 messages per second.
  
  The name of a FIFO queue must end with the .fifo suffix. The suffix counts towards the 80-character queue name limit. To determine whether a queue is FIFO, you can check whether the queue name ends with the suffix.
  
  If you have an existing application that uses standard queues and you want to take advantage of the ordering or exactly-once processing features of FIFO queues, you need to configure the queue and your application correctly. You can't convert an existing standard queue into a FIFO queue. To make the move, you must either create a new FIFO queue for your application or delete your existing standard queue and recreate it as a FIFO queue.
- Amazon EMR
  
  You can use Amazon Kinesis Data Firehose to load streaming data into data lakes, data stores, and analytics tools. It can capture, transform, and load streaming data into Amazon S3, Amazon Redshift, Amazon Elasticsearch Service, and Splunk.
- Use message timers to postpone the delivery of certain messages to the queue by one minute
  
  You can use message timers to set an initial invisibility period for a message added to a queue. So, if you send a message with a 60-second timer, the message isn't visible to consumers for its first 60 seconds in the queue. The default (minimum) delay for a message is 0 seconds. The maximum is 15 minutes. Therefore, you should use message timers to postpone the delivery of certain messages to the queue by one minute.
- "Use AWS Config to review resource configurations to meet compliance guidelines and maintain a history of resource configuration changes"
  
  AWS Config is a service that enables you to assess, audit, and evaluate the configurations of your AWS resources. With Config, you can review changes in configurations and relationships between AWS resources, dive into detailed resource configuration histories, and determine your overall compliance against the configurations specified in your internal guidelines. You can use Config to answer questions such as - “What did my AWS resource look like at xyz point in time?”.
- Upload the compressed file using multipart upload with S3 transfer acceleration
 
 Amazon S3 Transfer Acceleration enables fast, easy, and secure transfers of files over long distances between your client and an S3 bucket. Transfer Acceleration takes advantage of Amazon CloudFront’s globally distributed edge locations. As the data arrives at an edge location, data is routed to Amazon S3 over an optimized network path.
 
 Multipart upload allows you to upload a single object as a set of parts. Each part is a contiguous portion of the object's data. You can upload these object parts independently and in any order. If transmission of any part fails, you can retransmit that part without affecting other parts. After all parts of your object are uploaded, Amazon S3 assembles these parts and creates the object. If you're uploading large objects over a stable high-bandwidth network, use multipart uploading to maximize the use of your available bandwidth by uploading object parts in parallel for multi-threaded performance. If you're uploading over a spotty network, use multipart uploading to increase resiliency to network errors by avoiding upload restarts.
- Amazon S3 Glacier
  
  Amazon S3 Glacier is a secure, durable, and low-cost storage class for data archiving. Amazon S3 Glacier automatically encrypts data at rest using Advanced Encryption Standard (AES) 256-bit symmetric keys and supports secure transfer of your data over Secure Sockets Layer (SSL).
- If a spot request is persistent, then it is opened again after your Spot Instance is interrupted
  
  Spot blocks are designed not to be interrupted
  
  When you cancel an active spot request, it does not terminate the associated instance
  
  A Spot Instance is an unused EC2 instance that is available for less than the On-Demand price. Because Spot Instances enable you to request unused EC2 instances at steep discounts, you can lower your Amazon EC2 costs significantly. The hourly price for a Spot Instance is called a Spot price. The Spot price of each instance type in each Availability Zone is set by Amazon EC2 and adjusted gradually based on the long-term supply of and demand for Spot Instances.
  
  A Spot Instance request is either one-time or persistent. If the spot request is persistent, the request is opened again after your Spot Instance is interrupted. If the request is persistent and you stop your Spot Instance, the request only opens after you start your Spot Instance. Therefore the option - "If a spot request is persistent, then it is opened again after your Spot Instance is interrupted"
- The instances launched by both Launch Configuration LC1 and Launch Configuration LC2 will have dedicated instance tenancy
  
  A launch configuration is an instance configuration template that an Auto Scaling group uses to launch EC2 instances. When you create a launch configuration, you specify information for the instances. Include the ID of the Amazon Machine Image (AMI), the instance type, a key pair, one or more security groups, and a block device mapping. If you've launched an EC2 instance before, you specified the same information to launch the instance.
  
  When you create a launch configuration, the default value for the instance placement tenancy is null and the instance tenancy is controlled by the tenancy attribute of the VPC. If you set the Launch Configuration Tenancy to default and the VPC Tenancy is set to dedicated, then the instances have dedicated tenancy. If you set the Launch Configuration Tenancy to dedicated and the VPC Tenancy is set to default, then again the instances have dedicated tenancy.
- With cross-zone load balancing enabled, one instance in Availability Zone A receives 20% traffic and four instances in Availability Zone B receive 20% traffic each. With cross-zone load balancing disabled, one instance in Availability Zone A receives 50% traffic and four instances in Availability Zone B receive 12.5% traffic each
  
  The nodes for your load balancer distribute requests from clients to registered targets. When cross-zone load balancing is enabled, each load balancer node distributes traffic across the registered targets in all enabled Availability Zones. Therefore, one instance in Availability Zone A receives 20% traffic and four instances in Availability Zone B receive 20% traffic each. When cross-zone load balancing is disabled, each load balancer node distributes traffic only across the registered targets in its Availability Zone. Therefore, one instance in Availability Zone A receives 50% traffic and four instances in Availability Zone B receive 12.5% traffic each.
  
  Consider the following diagrams (the scenario illustrated in the diagrams involves 10 target instances split across 2 AZs) to understand the effect of cross-zone load balancing.
  
  If cross-zone load balancing is enabled, each of the 10 targets receives 10% of the traffic. This is because each load balancer node can route its 50% of the client traffic to all 10 targets.
- With cross-zone load balancing enabled, one instance in Availability Zone A receives 20% traffic and four instances in Availability Zone B receive 20% traffic each. With cross-zone load balancing disabled, one instance in Availability Zone A receives 50% traffic and four instances in Availability Zone B receive 12.5% traffic each
  
  The nodes for your load balancer distribute requests from clients to registered targets. When cross-zone load balancing is enabled, each load balancer node distributes traffic across the registered targets in all enabled Availability Zones. Therefore, one instance in Availability Zone A receives 20% traffic and four instances in Availability Zone B receive 20% traffic each. When cross-zone load balancing is disabled, each load balancer node distributes traffic only across the registered targets in its Availability Zone. Therefore, one instance in Availability Zone A receives 50% traffic and four instances in Availability Zone B receive 12.5% traffic each.
  
  Consider the following diagrams (the scenario illustrated in the diagrams involves 10 target instances split across 2 AZs) to understand the effect of cross-zone load balancing.
  
  If cross-zone load balancing is enabled, each of the 10 targets receives 10% of the traffic. This is because each load balancer node can route its 50% of the client traffic to all 10 targets.
- Internet Gateway (I1)
  
  An Internet Gateway is a horizontally scaled, redundant, and highly available VPC component that allows communication between your VPC and the internet.
  
  An Internet Gateway serves two purposes: to provide a target in your VPC route tables for internet-routable traffic and to perform network address translation (NAT) for instances that have been assigned public IPv4 addresses. Therefore, for instance E1, the Network Address Translation is done by Internet Gateway I1.
  
  Additionally, an Internet Gateway supports IPv4 and IPv6 traffic. It does not cause availability risks or bandwidth constraints on your network traffic.
  
  To enable access to or from the internet for instances in a subnet in a VPC, you must do the following:
  
  Attach an Internet gateway to your VPC.
  
  Add a route to your subnet's route table that directs internet-bound traffic to the internet gateway. If a subnet is associated with a route table that has a route to an internet gateway, it's known as a public subnet. If a subnet is associated with a route table that does not have a route to an internet gateway, it's known as a private subnet.
  
  Ensure that instances in your subnet have a globally unique IP address (public IPv4 address, Elastic IP address, or IPv6 address).
  
  Ensure that your network access control lists and security group rules allow the relevant traffic to flow to and from your instance.
- VPN CloudHub
  
  If you have multiple AWS Site-to-Site VPN connections, you can provide secure communication between sites using the AWS VPN CloudHub. This enables your remote sites to communicate with each other, and not just with the VPC. Sites that use AWS Direct Connect connections to the virtual private gateway can also be part of the AWS VPN CloudHub. The VPN CloudHub operates on a simple hub-and-spoke model that you can use with or without a VPC. This design is suitable if you have multiple branch offices and existing internet connections and would like to implement a convenient, potentially low-cost hub-and-spoke model for primary or backup connectivity between these remote offices.
  
  Per the given use-case, the corporate headquarters has an AWS Direct Connect connection to the VPC and the branch offices have Site-to-Site VPN connections to the VPC. Therefore using the AWS VPN CloudHub, branch offices can send and receive data with each other as well as with their corporate headquarters.
- Use Global Accelerator to provide a low latency way to ingest live sports results
  
  AWS Global Accelerator is a networking service that helps you improve the availability and performance of the applications that you offer to your global users. AWS Global Accelerator is easy to set up, configure, and manage. It provides static IP addresses that provide a fixed entry point to your applications and eliminate the complexity of managing specific IP addresses for different AWS Regions and Availability Zones. AWS Global Accelerator always routes user traffic to the optimal endpoint based on performance, reacting instantly to changes in application health, your user’s location, and policies that you configure. Global Accelerator is a good fit for non-HTTP use cases, such as gaming (UDP), IoT (MQTT), or Voice over IP.
- If a user or role has an IAM permission policy that grants access to an action that is either not allowed or explicitly denied by the applicable SCPs, the user or role can't perform that action
  
  SCPs affect all users and roles in attached accounts, including the root user
  
  SCPs do not affect service-linked role
  
  Service control policies (SCPs) are one type of policy that can be used to manage your organization. SCPs offer central control over the maximum available permissions for all accounts in your organization, allowing you to ensure your accounts stay within your organization’s access control guidelines.
  
  In SCPs, you can restrict which AWS services, resources, and individual API actions the users and roles in each member account can access. You can also define conditions for when to restrict access to AWS services, resources, and API actions. These restrictions even override the administrators of member accounts in the organization.
  
  Please note the following effects on permissions vis-a-vis the SCPs:
  
  If a user or role has an IAM permission policy that grants access to an action that is either not allowed or explicitly denied by the applicable SCPs, the user or role can't perform that action.
  
  SCPs affect all users and roles in the attached accounts, including the root user.
  
  SCPs do not affect any service-linked role.
- Use AWS CloudFormation StackSets to deploy the same template across AWS accounts and regions
  
  AWS CloudFormation StackSet extends the functionality of stacks by enabling you to create, update, or delete stacks across multiple accounts and regions with a single operation. A stack set lets you create stacks in AWS accounts across regions by using a single AWS CloudFormation template. Using an administrator account of an "AWS Organization", you define and manage an AWS CloudFormation template, and use the template as the basis for provisioning stacks into selected target accounts of an "AWS Organization" across specified regions.
- By default, basic monitoring is enabled when you use the AWS management console to create a launch configuration. Detailed monitoring is enabled by default when you create a launch configuration using the AWS CLI
  
  A launch configuration is an instance configuration template that an Auto Scaling group uses to launch EC2 instances. When you create a launch configuration, you specify information for the instances. Include the ID of the Amazon Machine Image (AMI), the instance type, a key pair, one or more security groups, and a block device mapping. If you've launched an EC2 instance before, you specified the same information to launch the instance.
  
  You configure monitoring for EC2 instances using a launch configuration or template. Monitoring is enabled whenever an instance is launched, either basic monitoring (5-minute granularity) or detailed monitoring (1-minute granularity). For detailed monitoring, additional charges apply.
  
  By default, basic monitoring is enabled when you create a launch template or when you use the AWS Management Console to create a launch configuration. Detailed monitoring is enabled by default when you create a launch configuration using the AWS CLI or an SDK.
- Use EC2 instances with Instance Store as the storage type
  
  An instance store provides temporary block-level storage for your instance. This storage is located on disks that are physically attached to the host computer. Instance store is ideal for the temporary storage of information that changes frequently, such as buffers, caches, scratch data, and other temporary content, or for data that is replicated across a fleet of instances, such as a load-balanced pool of web servers. Some instance types use NVMe or SATA-based solid-state drives (SSD) to deliver high random I/O performance. This is a good option when you need storage with very low latency, but you don't need the data to persist when the instance terminates or you can take advantage of fault-tolerant architectures.
  
  As Instance Store delivers high random I/O performance, it can act as a temporary storage space, and these volumes are included as part of the instance's usage cost, therefore this is the correct option.
- VPC with a public subnet only and AWS Site-to-Site VPN access
   VPC with public and private subnets and AWS Site-to-Site VPN access - The configuration for this scenario includes a virtual private cloud (VPC) with a public subnet and a private subnet, and a virtual private gateway to enable communication with your network over an IPsec VPN tunnel. We recommend this scenario if you want to extend your network into the cloud and also directly access the Internet from your VPC. This scenario enables you to run a multi-tiered application with a scalable web front end in a public subnet and to house your data in a private subnet that is connected to your network by an IPsec AWS Site-to-Site VPN connection
  
 - Data at rest inside the volume is encrypted
  
  Any snapshot created from the volume is encrypted
  
  Data moving between the volume and the instance is encrypted
  
  Amazon Elastic Block Store (Amazon EBS) provides block-level storage volumes for use with EC2 instances. When you create an encrypted EBS volume and attach it to a supported instance type, data stored at rest on the volume, data moving between the volume and the instance, snapshots created from the volume and volumes created from those snapshots are all encrypted. It uses AWS Key Management Service (AWS KMS) customer master keys (CMK) when creating encrypted volumes and snapshots. Encryption operations occur on the servers that host EC2 instances, ensuring the security of both data-at-rest and data-in-transit between an instance and its attached EBS storage.
- AWS Elastic Beanstalk is an easy-to-use service for deploying and scaling web applications and services developed with Java, .NET, PHP, Node.js, Python, Ruby, Go, and Docker on familiar servers such as Apache, Nginx, Passenger, and IIS. Simply upload your code and Elastic Beanstalk automatically handles the deployment, from capacity provisioning, load balancing, auto-scaling to application health monitoring. At the same time, you retain full control over the AWS resources powering your application and can access the underlying resources at any time. There is no additional charge for Elastic Beanstalk - you pay only for the AWS resources needed to store and run your applications.
- You can use an Internet Gateway ID as the custom source for the inbound rule
  
  A security group acts as a virtual firewall that controls the traffic for one or more instances. When you launch an instance, you can specify one or more security groups; otherwise, you can use the default security group. You can add rules to each security group that allows traffic to or from its associated instances. You can modify the rules for a security group at any time; the new rules are automatically applied to all instances that are associated with the security group.
- Use VPC sharing to share one or more subnets with other AWS accounts belonging to the same parent organization from AWS Organizations
  
  VPC sharing (part of Resource Access Manager) allows multiple AWS accounts to create their application resources such as EC2 instances, RDS databases, Redshift clusters, and Lambda functions, into shared and centrally-managed Amazon Virtual Private Clouds (VPCs). To set this up, the account that owns the VPC (owner) shares one or more subnets with other accounts (participants) that belong to the same organization from AWS Organizations. After a subnet is shared, the participants can view, create, modify, and delete their application resources in the subnets shared with them. Participants cannot view, modify, or delete resources that belong to other participants or the VPC owner.
  
  You can share Amazon VPCs to leverage the implicit routing within a VPC for applications that require a high degree of interconnectivity and are within the same trust boundaries. This reduces the number of VPCs that you create and manage while using separate accounts for billing and access control.
- Amazon FSx for Windows File Server - Amazon FSx for Windows File Server is a fully managed, highly reliable file storage that is accessible over the industry-standard Server Message Block (SMB) protocol. It is built on Windows Server, delivering a wide range of administrative features such as user quotas, end-user file restore, and Microsoft Active Directory (AD) integration.
  
  File Gateway Configuration of AWS Storage Gateway - Depending on the use case, Storage Gateway provides 3 types of storage interfaces for on-premises applications: File, Volume, and Tape. The File Gateway enables you to store and retrieve objects in Amazon S3 using file protocols such as Network File System (NFS) and Server Message Block (SMB).
- AWS Managed Microsoft AD
  
  AWS Directory Service provides multiple ways to use Amazon Cloud Directory and Microsoft Active Directory (AD) with other AWS services.
  
  AWS Directory Service for Microsoft Active Directory (aka AWS Managed Microsoft AD) is powered by an actual Microsoft Windows Server Active Directory (AD), managed by AWS. With AWS Managed Microsoft AD, you can run directory-aware workloads in the AWS Cloud such as SQL Server-based applications. You can also configure a trust relationship between AWS Managed Microsoft AD in the AWS Cloud and your existing on-premises Microsoft Active Directory, providing users and groups with access to resources in either domain, using single sign-on (SSO).
- You can change the tenancy of an instance from dedicated to host.You can change the tenancy of an instance from host to dedicated
Each EC2 instance that you launch into a VPC has a tenancy attribute. This attribute has the following values. By default, EC2 instances run on a shared-tenancy basis.
 A Dedicated Host is also a physical server that's dedicated to your use. With a Dedicated Host, you have visibility and control over how instances are placed on the server.
- Use RAID 0 when I/O performance is more important than fault tolerance
  
  With Amazon EBS, you can use any of the standard RAID configurations that you can use with a traditional bare metal server, as long as that particular RAID configuration is supported by the operating system for your instance. This is because all RAID is accomplished at the software level.
  
  RAID configuration options for I/O performance v/s fault tolerance:
- Use EC2 hibernate
  
  When you hibernate an instance, AWS signals the operating system to perform hibernation (suspend-to-disk). Hibernation saves the contents from the instance memory (RAM) to your Amazon EBS root volume. AWS then persists the instance's Amazon EBS root volume and any attached Amazon EBS data volumes.
  
  When you start your instance:
  
  The Amazon EBS root volume is restored to its previous state
  
  The RAM contents are reloaded
  
  The processes that were previously running on the instance are resumed
  
  Previously attached data volumes are reattached and the instance retains its instance ID
  
  Sicnce EC2 hibernate allows RAM contents to be reloaded and previously running processes to be resumed, hence this option is correct.
