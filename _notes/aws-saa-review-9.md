---
title: AWS SAA Practise test 9
layout: post
tags: [aws, saa, test]
date: 2021-03-11
--- 

- NAT Gateways deployed in your public subnet - You can use a network address translation (NAT) gateway to enable instances in a private subnet to connect to the internet or other AWS services, but prevent the internet from initiating a connection with those instances. A NAT gateway has the following characteristics and limitations:
  
  A NAT gateway supports 5 Gbps of bandwidth and automatically scales up to 45 Gbps.
  You can associate exactly one Elastic IP address with a NAT gateway.
  A NAT gateway supports the following protocols: TCP, UDP, and ICMP.
  You cannot associate a security group with a NAT gateway.
  You can use a network ACL to control the traffic to and from the subnet in which the NAT gateway is located.
  A NAT gateway can support up to 55,000 simultaneous connections to each unique destination.
  Therefore you must use a NAT Gateway in your public subnet in order to provide internet access to your instances in your private subnets. You are charged for creating and using a NAT gateway in your account. NAT gateway hourly usage and data processing rates apply.
- It authorizes an entire CIDR except one IP address to access the S3 bucket -
  
  You manage access in AWS by creating policies and attaching them to IAM identities (users, groups of users, or roles) or AWS resources. A policy is an object in AWS that, when associated with an identity or resource, defines their permissions. AWS evaluates these policies when an IAM principal (user or role) makes a request. Permissions in the policies determine whether the request is allowed or denied. Most policies are stored in AWS as JSON documents. AWS supports six types of policies: identity-based policies, resource-based policies, permissions boundaries, AWS Organizations SCPs, ACLs, and session policies.
- Move the data to S3 Standard IA after 30 days - S3 Standard-IA is for data that is accessed less frequently but requires rapid access when needed. S3 Standard-IA offers high durability, high throughput, and low latency of S3 Standard, with a low per GB storage price and per GB retrieval fee. This combination of low cost and high performance makes S3 Standard-IA ideal for long-term storage, backups, and as a data store for disaster recovery files. The minimum storage duration charge is 30 days.
  
  Analyze the cold data with Athena - Amazon Athena is an interactive query service that makes it easy to analyze data directly in Amazon S3 using standard SQL. Athena is serverless, so there is no infrastructure to set up or manage, and customers pay only for the queries they run. You can use Athena to process logs, perform ad-hoc analysis, and run interactive queries.
  
  Moving the data to S3 glacier will prevent us from being able to query it. Therefore, we should migrate the data to S3 Standard IA and use Athena to analyze the cold data.
- Amazon Neptune - Amazon Neptune is a fast, reliable, fully managed graph database service that makes it easy to build and run applications that work with highly connected datasets. The core of Amazon Neptune is a purpose-built, high-performance graph database engine optimized for storing billions of relationships and querying the graph with milliseconds latency. Neptune powers graph use cases such as recommendation engines, fraud detection, knowledge graphs, drug discovery, and network security.
  
  Amazon Neptune is highly available, with read replicas, point-in-time recovery, continuous backup to Amazon S3, and replication across Availability Zones. Neptune is secure with support for HTTPS encrypted client connections and encryption at rest. Neptune is fully managed, so you no longer need to worry about database management tasks such as hardware provisioning, software patching, setup, configuration, or backups.
  
  Amazon Neptune can quickly and easily process large sets of user-profiles and interactions to build social networking applications. Neptune enables highly interactive graph queries with high throughput to bring social features into your applications. For example, if you are building a social feed into your application, you can use Neptune to provide results that prioritize showing your users the latest updates from their family, from friends whose updates they ‘Like,’ and from friends who live close to them.
- Amazon EBS provides the various volume types, that differ in performance characteristics and price so that you can tailor your storage performance and cost to the needs of your applications. The volumes types fall into two categories:
  
  SSD-backed volumes optimized for transactional workloads involving frequent read/write operations with small I/O size, where the dominant performance attribute is IOPS.
  
  HDD-backed volumes optimized for large streaming workloads where throughput (measured in MiB/s) is a better performance measure than IOPS
  
  Provisioned IOPS SSD (io1) volumes are designed to meet the needs of I/O-intensive workloads, particularly database workloads, that are sensitive to storage performance and consistency. Unlike gp2, which uses a bucket and credit model to calculate performance, an io1 volume allows you to specify a consistent IOPS rate when you create the volume, and Amazon EBS delivers the provisioned performance 99.9 percent of the time.
  
  Convert the Amazon EC2 instance EBS volume to gp2 - General Purpose SSD (gp2) volumes offer cost-effective storage that is ideal for a broad range of workloads. These volumes deliver single-digit millisecond latencies and the ability to burst to 3,000 IOPS for an extended duration. Between a minimum of 100 IOPS (at 33.33 GiB and below) and a maximum of 16,000 IOPS (at 5,334 GiB and above), baseline performance scales linearly at 3 IOPS per GiB of volume size. AWS designs gp2 volumes to deliver a provisioned performance of 99% uptime. A gp2 volume can range in size from 1 GiB to 16 TiB.
  
  Therefore, gp2 is the right choice, allowing us to save on cost while keeping a burst in performance when needed.
- A VPC peering connection is a networking connection between two VPCs that enables you to route traffic between them using private IPv4 addresses or IPv6 addresses. Instances in either VPC can communicate with each other as if they are within the same network. You can create a VPC peering connection between your own VPCs, or with a VPC in another AWS account. The VPCs can be in different regions (also known as an inter-region VPC peering connection). VPC Peering helps connect two VPCs and is not transitive. To connect VPCs together, the best available option is to use VPC peering.
- Create an Application Load Balancer
  
  Application Load Balancer can automatically distribute incoming application traffic across multiple targets, such as Amazon EC2 instances, containers, IP addresses, and Lambda functions. It can handle the varying load of your application traffic in a single Availability Zone or across multiple Availability Zones.
  
  If your application is composed of several individual services, an Application Load Balancer can route a request to a service based on the content of the request.
  
  Here are the different types -
  
  Host-based Routing: You can route a client request based on the Host field of the HTTP header allowing you to route to multiple domains from the same load balancer. You can use host conditions to define rules that route requests based on the hostname in the host header (also known as host-based routing). This enables you to support multiple domains using a single load balancer. Example hostnames: example.com test.example.com *.example.com The rule *.example.com matches test.example.com but doesn't match example.com.
  
  Path-based Routing: You can route a client request based on the URL path of the HTTP header. You can use path conditions to define rules that route requests based on the URL in the request (also known as path-based routing). Example path patterns: /img/* /img//pics The path pattern is used to route requests but does not alter them. For example, if a rule has a path pattern of /img/, the rule would forward a request for /img/picture.jpg to the specified target group as a request for /img/picture.jpg. The path pattern is applied only to the path of the URL, not to its query parameters.
  
  HTTP header-based routing: You can route a client request based on the value of any standard or custom HTTP header.
  
  HTTP method-based routing: You can route a client request based on any standard or custom HTTP method.
  
  Query string parameter-based routing: You can route a client request based on query string or query parameters.
  
  Source IP address CIDR-based routing: You can route a client request based on source IP address CIDR from where the request originates.
  
  Path based routing and host based routing are only available for the Application Load Balancer (ALB). Therefore this is the correct option for the given use-case.
- AWS Elastic Beanstalk is an easy-to-use service for deploying and scaling web applications and services developed with Java, .NET, PHP, Node.js, Python, Ruby, Go, and Docker on familiar servers such as Apache, Nginx, Passenger, and IIS.
  
  You can simply upload your code and Elastic Beanstalk automatically handles the deployment, from capacity provisioning, load balancing, auto-scaling to application health monitoring. At the same time, you retain full control over the AWS resources powering your application and can access the underlying resources at any time.
  
  When you create an AWS Elastic Beanstalk environment, you can specify an Amazon Machine Image (AMI) to use instead of the standard Elastic Beanstalk AMI included in your platform version. A custom AMI can improve provisioning times when instances are launched in your environment if you need to install a lot of software that isn't included in the standard AMIs.
- Use Redis Auth - Amazon ElastiCache for Redis is a blazing fast in-memory data store that provides sub-millisecond latency to power internet-scale real-time applications.
  
  Amazon ElastiCache for Redis is a great choice for real-time transactional and analytical processing use cases such as caching, chat/messaging, gaming leaderboards, geospatial, machine learning, media streaming, queues, real-time analytics, and session store.
  
  ElastiCache for Redis supports replication, high availability, and cluster sharding right out of the box. IAM Auth is not supported by ElastiCache.
  
  Redis authentication tokens enable Redis to require a token (password) before allowing clients to execute commands, thereby improving data security.
- An Auto Scaling group (ASG) contains a collection of Amazon EC2 instances that are treated as a logical grouping for automatic scaling and management. An Auto Scaling group also enables you to use Amazon EC2 Auto Scaling features such as health check replacements and scaling policies. Both maintaining the number of instances in an Auto Scaling group and automatic scaling are the core functionality of the Amazon EC2 Auto Scaling service.
  
  Add a rule to authorize the security group of the ALB
  
  A security group acts as a virtual firewall that controls the traffic for one or more instances. When you launch an instance, you can specify one or more security groups; otherwise, we use the default security group. You can add rules to each security group that allow traffic to or from its associated instances. You can modify the rules for a security group at any time; the new rules are automatically applied to all instances that are associated with the security group. When deciding to allow traffic to reach an instance, all the rules from all the security groups that are associated with the instance are evaluated.
  
  The following are the characteristics of security group rules: 1. By default, security groups allow all outbound traffic. 2. Security group rules are always permissive; you can't create rules that deny access. 3. Security groups are stateful
  
  Application Load Balancer (ALB) operates at the request level (layer 7), routing traffic to targets – EC2 instances, containers, IP addresses and Lambda functions based on the content of the request. Ideal for advanced load balancing of HTTP and HTTPS traffic, Application Load Balancer provides advanced request routing targeted at delivery of modern application architectures, including microservices and container-based applications.
- Use IAM authentication from Lambda to RDS PostgreSQL
 
 Attach an AWS Identity and Access Management (IAM) role to AWS Lambda
 
 You can authenticate to your DB instance using AWS Identity and Access Management (IAM) database authentication. IAM database authentication works with MySQL and PostgreSQL. With this authentication method, you don't need to use a password when you connect to a DB instance. Instead, you use an authentication token.
 
 An authentication token is a unique string of characters that Amazon RDS generates on request. Authentication tokens are generated using AWS Signature Version 4. Each token has a lifetime of 15 minutes. You don't need to store user credentials in the database, because authentication is managed externally using IAM. You can also still use standard database authentication.
 
 IAM database authentication provides the following benefits: 1. Network traffic to and from the database is encrypted using Secure Sockets Layer (SSL). 2. You can use IAM to centrally manage access to your database resources, instead of managing access individually on each DB instance. 3. For applications running on Amazon EC2, you can use profile credentials specific to your EC2 instance to access your database instead of a password, for greater security.
- Warm Standby - The term warm standby is used to describe a DR scenario in which a scaled-down version of a fully functional environment is always running in the cloud. A warm standby solution extends the pilot light elements and preparation. It further decreases the recovery time because some services are always running. By identifying your business-critical systems, you can fully duplicate these systems on AWS and have them always on.
- The CNAME record will be updated to point to the standby DB - Amazon RDS provides high availability and failover support for DB instances using Multi-AZ deployments. Amazon RDS uses several different technologies to provide failover support. Multi-AZ deployments for MariaDB, MySQL, Oracle, and PostgreSQL DB instances use Amazon's failover technology. SQL Server DB instances use SQL Server Database Mirroring (DBM) or Always On Availability Groups (AGs).
  
  In a Multi-AZ deployment, Amazon RDS automatically provisions and maintains a synchronous standby replica in a different Availability Zone. The primary DB instance is synchronously replicated across Availability Zones to a standby replica to provide data redundancy, eliminate I/O freezes, and minimize latency spikes during system backups. Running a DB instance with high availability can enhance availability during planned system maintenance, and help protect your databases against DB instance failure and Availability Zone disruption.
  
  Failover is automatically handled by Amazon RDS so that you can resume database operations as quickly as possible without administrative intervention. When failing over, Amazon RDS simply flips the canonical name record (CNAME) for your DB instance to point at the standby, which is in turn promoted to become the new primary. Multi-AZ means the URL is the same, the failover is automated, and the CNAME will automatically be updated to point to the standby database.
- Set the minimum capacity to 2
  
  An Auto Scaling group contains a collection of Amazon EC2 instances that are treated as a logical grouping for automatic scaling and management. An Auto Scaling group also enables you to use Amazon EC2 Auto Scaling features such as health check replacements and scaling policies. Both maintaining the number of instances in an Auto Scaling group and automatic scaling are the core functionality of the Amazon EC2 Auto Scaling service.
  
  You configure the size of your Auto Scaling group by setting the minimum, maximum, and desired capacity. The minimum and maximum capacity are required to create an Auto Scaling group, while the desired capacity is optional. If you do not define your desired capacity upfront, it defaults to your minimum capacity.
  
  An Auto Scaling group is elastic as long as it has different values for minimum and maximum capacity. All requests to change the Auto Scaling group's desired capacity (either by manual scaling or automatic scaling) must fall within these limits.
  
  Here, even though our ASG is deployed across 3 AZs, the minimum capacity to be highly available is 2. When we specify 2 as the minimum capacity, the ASG would create these 2 instances in separate AZs. If demand goes up, the ASG would spin up a new instance in the third AZ. Later as the demand subsides, the ASG would scale-in and the instance count would be back to 2.
  
  Use Reserved Instances for the minimum capacity
  
  Reserved Instances provide you with significant savings on your Amazon EC2 costs compared to On-Demand Instance pricing. Reserved Instances are not physical instances, but rather a billing discount applied to the use of On-Demand Instances in your account. These On-Demand Instances must match certain attributes, such as instance type and Region, to benefit from the billing discount. Since minimum capacity will always be maintained, it is cost-effective to choose reserved instances than any other option.
  
  In case of an AZ outage, the instance in that AZ would go down however the other instance would still be available. The ASG would provision the replacement instance in the third AZ to keep the minimum count to 2.
- Use multi-part upload feature of Amazon S3 - Multi-part upload allows you to upload a single object as a set of parts. Each part is a contiguous portion of the object's data. You can upload these object parts independently and in any order. If transmission of any part fails, you can retransmit that part without affecting other parts. After all parts of your object are uploaded, Amazon S3 assembles these parts and creates the object.
  
  AWS recommends that you use multi-part uploading in the following ways: If you're uploading large objects over a stable high-bandwidth network, use multi-part uploading to maximize the use of your available bandwidth by uploading object parts in parallel for multi-threaded performance. If you're uploading over a spotty network, use multi-part uploading to increase resiliency to network errors by avoiding upload restarts. When using multi-part uploading, you need to retry uploading only parts that are interrupted during the upload. You don't need to restart uploading your object from the beginning.
  
  In general, when your object size reaches 100 MB, you should consider using multipart uploads instead of uploading the object in a single operation. If the file is greater than 5GB in size, you must use multi-part upload to upload that file to S3.
- Application Load Balancer + dynamic port mapping
  
  Application Load Balancer can automatically distribute incoming application traffic across multiple targets, such as Amazon EC2 instances, containers, IP addresses, and Lambda functions. It can handle the varying load of your application traffic in a single Availability Zone or across multiple Availability Zones.
  
  Dynamic port mapping with an Application Load Balancer makes it easier to run multiple tasks on the same Amazon ECS service on an Amazon ECS cluster.
- To manage your S3 objects, so they are stored cost-effectively throughout their lifecycle, configure their Amazon S3 Lifecycle. An S3 Lifecycle configuration is a set of rules that define actions that Amazon S3 applies to a group of objects. There are two types of actions:
  
  Transition actions — Define when objects transition to another storage class. For example, you might choose to transition objects to the S3 Standard-IA storage class 30 days after you created them, or archive objects to the S3 Glacier storage class one year after creating them.
  
  Expiration actions — Define when objects expire. Amazon S3 deletes expired objects on your behalf.
  
  Create a Lifecycle Policy to transition objects to S3 Standard IA using a prefix after 45 days
  
  S3 Standard-IA is for data that is accessed less frequently but requires rapid access when needed. S3 Standard-IA offers high durability, high throughput, and low latency of S3 Standard, with a low per GB storage price and per GB retrieval fee. This combination of low cost and high performance makes S3 Standard-IA ideal for long-term storage, backups, and as a data store for disaster recovery files. The minimum storage duration charge is 30 days.
  
  As the use-case mentions that after 45 days, image files are rarely requested, but the thumbnails still are. So you need to use a prefix while configuring the Lifecycle Policy so that only objects in the s3://my-bucket/images are transitioned to Standard IA and not all the objects in the bucket.
  
  Create a Lifecycle Policy to transition all objects to Glacier after 180 days
  
  Amazon S3 Glacier and S3 Glacier Deep Archive are secure, durable, and extremely low-cost Amazon S3 cloud storage classes for data archiving and long-term backup. They are designed to deliver 99.999999999% durability, and provide comprehensive security and compliance capabilities that can help meet even the most stringent regulatory requirements.
- Amazon Route 53 is a highly available and scalable cloud Domain Name System (DNS) web service. Amazon Route 53 effectively connects user requests to infrastructure running in AWS – such as Amazon EC2 instances, Elastic Load Balancing load balancers, or Amazon S3 buckets – and can also be used to route users to infrastructure outside of AWS.
  
  You can use Amazon Route 53 to configure DNS health checks to route traffic to healthy endpoints or to independently monitor the health of your application and its endpoints. Amazon Route 53 Traffic Flow makes it easy for you to manage traffic globally through a variety of routing types, including Latency Based Routing, Geo DNS, Geoproximity, and Weighted Round Robin—all of which can be combined with DNS Failover to enable a variety of low-latency, fault-tolerant architectures.
  
  The TTL is still in effect - TTL (time to live), is the amount of time, in seconds, that you want DNS recursive resolvers to cache information about a record. If you specify a longer value (for example, 172800 seconds, or two days), you reduce the number of calls that DNS recursive resolvers must make to Route 53 to get the latest information for the record. This has the effect of reducing latency and reducing your bill for Route 53 service.
  
  However, if you specify a longer value for TTL, it takes longer for changes to the record (for example, a new IP address) to take effect because recursive resolvers use the values in their cache for longer periods before they ask Route 53 for the latest information. If you're changing settings for a domain or subdomain that's already in use, AWS recommends that you initially specify a shorter value, such as 300 seconds, and increase the value after you confirm that the new settings are correct.
  
  For this use-case, the most likely issue is that the TTL is still in effect so you have to wait until it expires for the new request to perform another DNS query and get the value for the new Load Balancer.
- Add a rule authorizing the EC2 security group
  
  A security group acts as a virtual firewall that controls the traffic for one or more instances. When you launch an instance, you can specify one or more security groups; otherwise, we use the default security group. You can add rules to each security group that allow traffic to or from its associated instances. You can modify the rules for a security group at any time; the new rules are automatically applied to all instances that are associated with the security group. When we decide whether to allow traffic to reach an instance, we evaluate all the rules from all the security groups that are associated with the instance. The following are the characteristics of security group rules: By default, security groups allow all outbound traffic. Security group rules are always permissive; you can't create rules that deny access. Security groups are stateful.
  
  In our scenario, the EC2 instances that are part of the ASG are the ones accessing the database layer. The correct response is to add a rule to the security group attached to Aurora authorizing the EC2 instance's security group.
- Use AWS Lambda to generate the URL
  
  Generate a CloudFront signed URL
  
  To restrict access to content that you serve from Amazon S3 buckets, follow these steps:
  
  Create a special CloudFront user called an origin access identity (OAI) and associate it with your distribution. Configure your S3 bucket permissions so that CloudFront can use the OAI to access the files in your bucket and serve them to your users. Make sure that users can’t use a direct URL to the S3 bucket to access a file there.
  
  After you take these steps, users can only access your files through CloudFront, not directly from the S3 bucket. In general, if you’re using an Amazon S3 bucket as the origin for a CloudFront distribution, you can either allow everyone to have access to the files there, or you can restrict access. If you restrict access by using, for example, CloudFront signed URLs or signed cookies, you also won’t want people to be able to view files by simply using the direct Amazon S3 URL for the file. Instead, you want them to only access the files by using the CloudFront URL, so your content remains protected.
  
  To generate this URL we must code, and Lambda is the perfect tool for running that code on the fly.
- Amazon DynamoDB is a key-value and document database that delivers single-digit millisecond performance at any scale. It's a fully managed, multi-Region, multi-master, durable database with built-in security, backup and restore, and in-memory caching for internet-scale applications.
  
  DynamoDB Streams + Lambda - A DynamoDB stream is an ordered flow of information about changes to items in a DynamoDB table. When you enable a stream on a table, DynamoDB captures information about every modification to data items in the table. Whenever an application creates, updates, or deletes items in the table, DynamoDB Streams writes a stream record with the primary key attributes of the items that were modified. A stream record contains information about a data modification to a single item in a DynamoDB table.
  
  DynamoDB Streams will contain a stream of all the changes that happen to a DynamoDB table. It can be chained with a Lambda function that will be triggered to react to these changes, one of which is the developer's milestone. Therefore, this is the correct option.
- Amazon Elastic Compute Cloud (Amazon EC2) is a web service that provides secure, resizable compute capacity in the cloud. It is designed to make web-scale cloud computing easier for developers. Amazon EC2’s simple web service interface allows you to obtain and configure capacity with minimal friction. It provides you with complete control of your computing resources and lets you run on Amazon’s proven computing environment. AWS Batch can be used to plan, schedule, and execute your batch computing workloads on Amazon EC2 Instances. Amazon EC2 is the right choice as it can accommodate batch processing and run customized scripts, as is the needed requirement.
- EC2 Spot Instances - A Spot Instance is an unused EC2 instance that is available for less than the On-Demand price. Because Spot Instances enable you to request unused EC2 instances at steep discounts, you can lower your Amazon EC2 costs significantly. The hourly price for a Spot Instance is called a Spot price. The Spot price of each instance type in each Availability Zone is set by Amazon EC2 and adjusted gradually based on the long-term supply of and demand for Spot Instances. Your Spot Instance runs whenever capacity is available and the maximum price per hour for your request exceeds the Spot price.
  
  To process these jobs, due to the unpredictable nature of their volume, and the desire to save on costs, spot Instances are recommended as compared to on-demand instances. As spot instances are cheaper than reserved instances and do not require long term commitment, spot instances are a better fit for the given use-case. Amazon Simple Queue Service (SQS) - Amazon Simple Queue Service (SQS) is a fully managed message queuing service that enables you to decouple and scale microservices, distributed systems, and serverless applications. SQS offers two types of message queues. Standard queues offer maximum throughput, best-effort ordering, and at-least-once delivery. SQS FIFO queues are designed to guarantee that messages are processed exactly once, in the exact order that they are sent.
                                                                                                                                                                                                                                                                                                                                     
                                                                                                                                                                                                                                                                                                                                     SQS will allow you to buffer the image compression requests and process them asynchronously. It also has a direct built-in mechanism for retries and scales seamlessly.
- Create an origin access identity (OAI) and update the S3 Bucket Policy
  
  To restrict access to content that you serve from Amazon S3 buckets, you need to follow the following steps:
  
  Create a special CloudFront user called an origin access identity (OAI) and associate it with your distribution.
  Configure your S3 bucket permissions so that CloudFront can use the OAI to access the files in your bucket and serve them to your users. Make sure that users can’t use a direct URL to the S3 bucket to access a file there.
  After you take these steps, users can only access your files through CloudFront, not directly from the S3 bucket.
  
  In general, if you’re using an Amazon S3 bucket as the origin for a CloudFront distribution, you can either allow everyone to have access to the files there, or you can restrict access. If you restrict access by using, for example, CloudFront signed URLs or signed cookies, you also won’t want people to be able to view files by simply using the direct Amazon S3 URL for the file. Instead, you want them to only access the files by using the CloudFront URL, so your content remains protected.
- Create a new launch configuration with the new AMI ID
  
  An Auto Scaling group contains a collection of Amazon EC2 instances that are treated as a logical grouping for the purposes of automatic scaling and management. An Auto Scaling group also enables you to use Amazon EC2 Auto Scaling features such as health check replacements and scaling policies.
  
  A launch configuration is an instance configuration template that an Auto Scaling group uses to launch EC2 instances. When you create a launch configuration, you specify information for the instances. Include the ID of the Amazon Machine Image (AMI), the instance type, a key pair, one or more security groups, and a block device mapping. If you've launched an EC2 instance before, you specified the same information in order to launch the instance.
  
  An Auto Scaling group is associated with one launch configuration at a time, and you can't modify a launch configuration after you've created it. To change the launch configuration for an Auto Scaling group, use an existing launch configuration as the basis for a new launch configuration. Then, update the Auto Scaling group to use the new launch configuration.
  
  After you change the launch configuration for an Auto Scaling group, any new instances are launched using the new configuration options, but existing instances are not affected. To update the existing instances, terminate them so that they are replaced by your Auto Scaling group, or allow automatic scaling to gradually replace older instances with newer instances based on your termination policies.
- It allows any IP to pass through on the HTTP port
  
  It configures a security group's inbound rules
  
  It lets traffic flow from one IP on port 22
  
  A security group acts as a virtual firewall that controls the traffic for one or more instances. When you launch an instance, you can specify one or more security groups; otherwise, we use the default security group. You can add rules to each security group that allows traffic to or from its associated instances. You can modify the rules for a security group at any time; the new rules are automatically applied to all instances that are associated with the security group. When we decide whether to allow traffic to reach an instance, we evaluate all the rules from all the security groups that are associated with the instance.
  
  The following are the characteristics of security group rules: 1. By default, security groups allow all outbound traffic. 2. Security group rules are always permissive; you can't create rules that deny access. 3. Security groups are stateful
  
  AWS CloudFormation provides a common language for you to model and provision AWS and third-party application resources in your cloud environment. AWS CloudFormation allows you to use programming languages or a simple text file to model and provision, in an automated and secure manner, all the resources needed for your applications across all regions and accounts. This gives you a single source of truth for your AWS and third-party resources.
  
  Considering the given CloudFormation snippet, 0.0.0.0/0 means any IP, not the IP 0.0.0.0. Ingress means traffic going into your instance, and Security Groups are different from NACL. Each "-" in our security group rule represents a different rule (YAML syntax)
  
  Therefore the CloudFormation snippet creates two Security Group inbound rules that allow any IP to pass through on the HTTP port and lets traffic flow from one source IP (192.168.1.1) on port 22.
- Enable API Gateway Caching - Amazon API Gateway is a fully managed service that makes it easy for developers to create, publish, maintain, monitor, and secure APIs at any scale. APIs act as the "front door" for applications to access data, business logic, or functionality from your backend services. Using API Gateway, you can create RESTful APIs and WebSocket APIs that enable real-time two-way communication applications. API Gateway supports containerized and serverless workloads, as well as web applications.
  
  You can enable API caching in Amazon API Gateway to cache your endpoint's responses. With caching, you can reduce the number of calls made to your endpoint and also improve the latency of requests to your API. When you enable caching for a stage, API Gateway caches responses from your endpoint for a specified time-to-live (TTL) period, in seconds. API Gateway then responds to the request by looking up the endpoint response from the cache instead of requesting your endpoint. The default TTL value for API caching is 300 seconds. The maximum TTL value is 3600 seconds. TTL=0 means caching is disabled. Using API Gateway Caching feature is the answer for the use case, as we can accept stale data for about 24 hours.
- Pilot Light - The term pilot light is often used to describe a DR scenario in which a minimal version of an environment is always running in the cloud. The idea of the pilot light is an analogy that comes from the gas heater. In a gas heater, a small flame that’s always on can quickly ignite the entire furnace to heat up a house. This scenario is similar to a backup-and-restore scenario. For example, with AWS you can maintain a pilot light by configuring and running the most critical core elements of your system in AWS. For the given use-case, a small part of the backup infrastructure is always running simultaneously syncing mutable data (such as databases or documents) so that there is no loss of critical data. When the time comes for recovery, you can rapidly provision a full-scale production environment around the critical core.
- SSE-C - With Server-Side Encryption with Customer-Provided Keys (SSE-C), you manage the encryption keys and Amazon S3 manages the encryption, as it writes to disks, and decryption when you access your objects. With SSE-C, the startup can still provide the encryption key but let AWS do the encryption. Therefore, this is the correct option.