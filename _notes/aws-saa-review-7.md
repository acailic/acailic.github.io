---
title: AWS SAA Practise test 7
layout: post
tags: [aws, saa, test]
date: 2020-08-17
--- 
 
- Setup another fleet of EC2 instances for the web tier in the eu-west-1 region. Enable latency routing policy in Route 53 - Amazon Route 53 is a highly available and scalable cloud Domain Name System (DNS) web service. Use latency based routing when you have resources in multiple AWS Regions and you want to route traffic to the region that provides the lowest latency. To use latency-based routing, you create latency records for your resources in multiple AWS Regions. When Route 53 receives a DNS query for your domain or subdomain (example.com or acme.example.com), it determines which AWS Regions you've created latency records for, determines which region gives the user the lowest latency, and then selects a latency record for that region. Route 53 responds with the value from the selected record, such as the IP address for a web server.
  
  As customers in Europe are facing performance issues with high application load time, you can use latency based routing to reduce the latency. 
- Because of a configuration issue, the consumer application is not deleting the messages in the SQS queue after it has processed them
  
  Amazon Simple Queue Service (SQS) is a fully managed message queuing service that enables you to decouple and scale microservices, distributed systems, and serverless applications.
  
  It is the consumer application's responsibility to process the message from the queue and delete them once the processing is done. Otherwise, the message will be processed repeatedly by consumer applications. The SQS queue will not delete any messages unless the default retention period of 4 days is over. This is to make sure that the message is still available for processing by another consumer, in case the first consumer application fails while it is still processing the message.
  
  If the consumer application is misconfigured to not delete messages after processing, then we can see this issue as described in the use-case. Hence, this is the correct option.
- The security group of RDS should have an inbound rule from the security group of the EC2 instances in the ASG on port 5432
  
  The security group of the EC2 instances should have an inbound rule from the security group of the ALB on port 80
  
  The security group of the ALB should have an inbound rule from anywhere on port 443
  
  A security group acts as a virtual firewall that controls the traffic for one or more instances. When you launch an instance, you can specify one or more security groups; otherwise, we use the default security group. You can add rules to each security group that allows traffic to or from its associated instances. You can modify the rules for a security group at any time; the new rules are automatically applied to all instances that are associated with the security group. When we decide whether to allow traffic to reach an instance, we evaluate all the rules from all the security groups that are associated with the instance. The following are the characteristics of security group rules: By default, security groups allow all outbound traffic. Security group rules are always permissive; you can't create rules that deny access. Security groups are stateful
  
  PostgreSQL port = 5432 HTTP port = 80 HTTPS port = 443
  
  The traffic goes like this : The client sends an HTTPS request to ALB on port 443. This is handled by the rule - The security group of the ALB should have an inbound rule from anywhere on port 443. The ALB then forwards the request to one of the EC2 instances. This is handled by the rule - The security group of the EC2 instances should have an inbound rule from the security group of the ALB on port 80. The EC2 instance further accesses the PostgreSQL database managed by RDS on port 5432. This is handled by the rule - The security group of RDS should have an inbound rule from the security group of the EC2 instances in the ASG on port 5432.
- Create a Read Replica in the same AZ as the Master database and point the analytics workload there
  
  Amazon RDS Read Replicas provide enhanced performance and durability for RDS database (DB) instances. They make it easy to elastically scale out beyond the capacity constraints of a single DB instance for read-heavy database workloads. For the MySQL, MariaDB, PostgreSQL, Oracle, and SQL Server database engines, Amazon RDS creates a second DB instance using a snapshot of the source DB instance. It then uses the engines' native asynchronous replication to update the read replica whenever there is a change to the source DB instance. Read replicas can be within an Availability Zone, Cross-AZ, or Cross-Region.
  
  Creating a Read Replica is the answer. As we want to minimize the costs, we need to launch the Read Replica in the same AZ, because we have to pay for inter-AZ data transfer, whereas the transfer of data within a single AZ is free.
- You can only use a launch template to provision capacity across multiple instance types using both On-Demand Instances and Spot Instances to achieve the desired scale, performance, and cost
  
  A launch template is similar to a launch configuration, in that it specifies instance configuration information such as the ID of the Amazon Machine Image (AMI), the instance type, a key pair, security groups, and the other parameters that you use to launch EC2 instances. Also, defining a launch template instead of a launch configuration allows you to have multiple versions of a template.
  
  With launch templates, you can provision capacity across multiple instance types using both On-Demand Instances and Spot Instances to achieve the desired scale, performance, and cost.
- Run on a Spot Instance with Spot Block
  
  A Spot Instance is an unused EC2 instance that is available for less than the On-Demand price. Because Spot Instances enable you to request unused EC2 instances at steep discounts, you can lower your Amazon EC2 costs significantly. The hourly price for a Spot Instance is called a Spot price.
  
  Spot Instances with a defined duration (also known as Spot blocks) are designed not to be interrupted and will run continuously for the duration you select. This makes them ideal for jobs that take a finite time to complete, such as batch processing, encoding and rendering, modeling and analysis, and continuous integration.
  
  Running our load on a Spot Instance with Spot Block sounds like the perfect use case, as we can block the spot instance for 1 hour, run the script there, and then the instance will be terminated.
- **Kinesis Agent cannot write to a Kinesis Firehose for which the delivery stream source is already set as Kinesis Data
  
  Amazon Kinesis Data Firehose is the easiest way to reliably load streaming data into data lakes, data stores, and analytics tools. It is a fully managed service that automatically scales to match the throughput of your data and requires no ongoing administration. It can also batch, compress, transform, and encrypt the data before loading it, minimizing the amount of storage used at the destination and increasing security.
  
  When a Kinesis data stream is configured as the source of a Firehose delivery stream, Firehose’s PutRecord and PutRecordBatch operations are disabled and Kinesis Agent cannot write to Firehose delivery stream directly. Data needs to be added to the Kinesis data stream through the Kinesis Data Streams PutRecord and PutRecords operations instead. Therefore, this option is correct.
- Standard SQS queue is only allowed as an Amazon S3 event notification destination, whereas FIFO SQS queue is not allowed
  
  The Amazon S3 notification feature enables you to receive notifications when certain events happen in your bucket. To enable notifications, you must first add a notification configuration that identifies the events you want Amazon S3 to publish and the destinations where you want Amazon S3 to send the notifications.
  
  Amazon S3 supports the following destinations where it can publish events:
  
  Amazon Simple Notification Service (Amazon SNS) topic
  
  Amazon Simple Queue Service (Amazon SQS) queue
  
  AWS Lambda
  
  Currently, the Standard SQS queue is only allowed as an Amazon S3 event notification destination, whereas the FIFO SQS queue is not allowed.
- AWS Secrets Manager helps you protect secrets needed to access your applications, services, and IT resources. The service enables you to easily rotate, manage, and retrieve database credentials, API keys, and other secrets throughout their lifecycle. Users and applications retrieve secrets with a call to Secrets Manager APIs, eliminating the need to hardcode sensitive information in plain text. Secrets Manager offers secret rotation with built-in integration for Amazon RDS, Amazon Redshift, and Amazon DocumentDB.
- Increase the deregistration delay to more than 10 minutes
  
  Elastic Load Balancing stops sending requests to targets that are deregistering. By default, Elastic Load Balancing waits 300 seconds before completing the deregistration process, which can help in-flight requests to the target to complete. We need to update this value to more than 10 minutes to allow our tax software to complete in-flight requests. Therefore this is the correct option.
- Remove the member account from the old organization. Send an invite to the new organization. Accept the invite to the new organization from the member account
  
  AWS Organizations helps you centrally govern your environment as you grow and scale your workloads on AWS. Using AWS Organizations, you can automate account creation, create groups of accounts to reflect your business needs, and apply policies for these groups for governance. You can also simplify billing by setting up a single payment method for all of your AWS accounts. Through integrations with other AWS services, you can use Organizations to define central configurations and resource sharing across accounts in your organization.
  
  To migrate accounts from one organization to another, you must have root or IAM access to both the member and master accounts. Here are the steps to follow: 1. Remove the member account from the old organization 2. Send an invite to the new organization 3. Accept the invite to the new organization from the member account
- Write a one time job to copy the videos from all EBS volumes to S3 and then modify the application to use Amazon S3 standard for storing the videos
  
  Mount EFS on all EC2 instances. Write a one time job to copy the videos from all EBS volumes to EFS. Modify the application to use EFS for storing the videos
  
  Amazon Elastic Block Store (EBS) is an easy to use, high-performance block storage service designed for use with Amazon Elastic Compute Cloud (EC2) for both throughput and transaction-intensive workloads at any scale.
  
  Amazon Elastic File System (Amazon EFS) provides a simple, scalable, fully managed elastic NFS file system for use with AWS Cloud services and on-premises resources. It is built to scale on-demand to petabytes without disrupting applications, growing and shrinking automatically as you add and remove files, eliminating the need to provision and manage capacity to accommodate growth.
  
  Amazon Simple Storage Service (Amazon S3) is an object storage service that offers industry-leading scalability, data availability, security, and performance.
  
  As EBS volumes are attached locally to the EC2 instances, therefore the uploaded videos are tied to specific EC2 instances. Every time the user logs in, they are directed to a different instance and therefore their videos get dispersed across multiple EBS volumes. The correct solution is to use either S3 or EFS to store the user videos.
- Amazon ElastiCache for Redis is a blazing fast in-memory data store that provides sub-millisecond latency to power internet-scale real-time applications. Amazon ElastiCache for Redis is a great choice for real-time transactional and analytical processing use cases such as caching, chat/messaging, gaming leaderboards, geospatial, machine learning, media streaming, queues, real-time analytics, and session store. ElastiCache for Redis supports replication, high availability, and cluster sharding right out of the box. Amazon ElastiCache for Redis is also HIPAA Eligible Service. Therefore, this is the correct option.
- Chef
  
  Puppet
  
  AWS OpsWorks is a configuration management service that provides managed instances of Chef and Puppet. Chef and Puppet are automation platforms that allow you to use code to automate the configurations of your servers. OpsWorks lets you use Chef and Puppet to automate how servers are configured, deployed and managed across your Amazon EC2 instances or on-premises compute environments.
- EFS IA
  
  Amazon Elastic File System (Amazon EFS) provides a simple, scalable, fully managed elastic NFS file system for use with AWS Cloud services and on-premises resources. Amazon EFS is a regional service storing data within and across multiple Availability Zones (AZs) for high availability and durability.
  
  Amazon EFS Infrequent Access (EFS IA) is a storage class that provides price/performance that is cost-optimized for files, not accessed every day, with storage prices up to 92% lower compared to Amazon EFS Standard. Therefore, this is the correct option.
- Take a snapshot of the database, copy it as an encrypted snapshot, and restore a database from the encrypted snapshot. Terminate the previous database
  
  Amazon Relational Database Service (Amazon RDS) makes it easy to set up, operate, and scale a relational database in the cloud. It provides cost-efficient and resizable capacity while automating time-consuming administration tasks such as hardware provisioning, database setup, patching and backups.
  
  You can encrypt your Amazon RDS DB instances and snapshots at rest by enabling the encryption option for your Amazon RDS DB instances. Data that is encrypted at rest includes the underlying storage for DB instances, its automated backups, read replicas, and snapshots.
  
  You can only enable encryption for an Amazon RDS DB instance when you create it, not after the DB instance is created. However, because you can encrypt a copy of an unencrypted DB snapshot, you can effectively add encryption to an unencrypted DB instance. That is, you can create a snapshot of your DB instance, and then create an encrypted copy of that snapshot. So this is the correct option.
- Run the workload on a Spot Fleet
  
  The Spot Fleet selects the Spot Instance pools that meet your needs and launches Spot Instances to meet the target capacity for the fleet. By default, Spot Fleets are set to maintain target capacity by launching replacement instances after Spot Instances in the fleet are terminated.
  
  A Spot Instance is an unused EC2 instance that is available for less than the On-Demand price. Spot Instances provide great cost efficiency, but we need to select an instance type in advance. In this case, we want to use the most cost-optimal option and leave the selection of the cheapest spot instance to a Spot Fleet request, which can be optimized with the lowestPrice strategy. So this is the correct option.
- Create a public Network Load Balancer that links to EC2 instances that are bastion hosts managed by an ASG
  
  Network Load Balancer is best suited for use-cases involving low latency and high throughput workloads that involve scaling to millions of requests per second. Network Load Balancer operates at the connection level (Layer 4), routing connections to targets - Amazon EC2 instances, microservices, and containers – within Amazon Virtual Private Cloud (Amazon VPC) based on IP protocol data.
  
  Including bastion hosts in your VPC environment enables you to securely connect to your Linux instances without exposing your environment to the Internet. After you set up your bastion hosts, you can access the other instances in your VPC through Secure Shell (SSH) connections on Linux. Bastion hosts are also configured with security groups to provide fine-grained ingress control.
  
  You need to remember that Bastion Hosts are using the SSH protocol, which is a TCP based protocol on port 22. They must be publicly accessible.
  
  Here, the correct answer is to use a Network Load Balancer, which supports TCP traffic, and will automatically allow you to connect to the EC2 instance in the backend.
- Use an SQS FIFO queue, and make sure the telemetry data is sent with a Group ID attribute representing the value of the Desktop ID
  
  Amazon Simple Queue Service (SQS) is a fully managed message queuing service that enables you to decouple and scale microservices, distributed systems, and serverless applications. SQS offers two types of message queues. Standard queues offer maximum throughput, best-effort ordering, and at-least-once delivery. SQS FIFO queues are designed to guarantee that messages are processed exactly once, in the exact order that they are sent.
  
  We, therefore, need to use an SQS FIFO queue. If we don't specify a GroupID, then all the messages are in absolute order, but we can only have 1 consumer at most. To allow for multiple consumers to read data for each Desktop application, and to scale the number of consumers, we should use the "Group ID" attribute. So this is the correct option.
- http://bucket-name.s3-website-Region.amazonaws.com
  
  To host a static website on Amazon S3, you configure an Amazon S3 bucket for website hosting and then upload your website content to the bucket. When you configure a bucket as a static website, you enable static website hosting, set permissions, and add an index document. Depending on your website requirements, you can also configure other options, including redirects, web traffic logging, and custom error documents.
  
  When you configure your bucket as a static website, the website is available at the AWS Region-specific website endpoint of the bucket.
  
  Depending on your Region, your Amazon S3 website endpoints follow one of these two formats.
  
  s3-website dash (-) Region ‐ http://bucket-name.s3-website.Region.amazonaws.com
  
  s3-website dot (.) Region ‐ http://bucket-name.s3-website-Region.amazonaws.com
  
  These URLs return the default index document that you configure for the website.
- Kinesis Data Firehose
  
  Amazon Kinesis Data Firehose is the easiest way to reliably load streaming data into data lakes, data stores, and analytics tools. It can capture, transform, and load streaming data into Amazon S3, Amazon Redshift, Amazon Elasticsearch Service, and Splunk, enabling near real-time analytics with existing business intelligence tools and dashboards you’re already using today. It is a fully managed service that automatically scales to match the throughput of your data and requires no ongoing administration. Therefore, this is the correct option.
- Partition placement group
  
  You can use placement groups to influence the placement of a group of interdependent instances to meet the needs of your workload. Depending on the type of workload, you can create a placement group using one of the following placement strategies:
  
  Partition – spreads your instances across logical partitions such that groups of instances in one partition do not share the underlying hardware with groups of instances in different partitions. This strategy is typically used by large distributed and replicated workloads, such as Hadoop, Cassandra, and Kafka. Therefore, this is the correct option for the given use-case.
- Create an IP match condition in the WAF to block the malicious IP address
  
  AWS WAF is a web application firewall that helps protect your web applications or APIs against common web exploits that may affect availability, compromise security, or consume excessive resources. AWS WAF gives you control over how traffic reaches your applications by enabling you to create security rules that block common attack patterns, such as SQL injection or cross-site scripting, and rules that filter out specific traffic patterns you define.
- A process replaces an existing object and immediately tries to read it. Amazon S3 always returns the latest version of the object
  
  Amazon S3 delivers strong read-after-write consistency automatically, without changes to performance or availability, without sacrificing regional isolation for applications, and at no additional cost.
  
  After a successful write of a new object or an overwrite of an existing object, any subsequent read request immediately receives the latest version of the object. S3 also provides strong consistency for list operations, so after a write, you can immediately perform a listing of the objects in a bucket with any changes reflected.
  
  Strong read-after-write consistency helps when you need to immediately read an object after a write. For example, strong read-after-write consistency when you often read and list immediately after writing objects.
  
  To summarize, all S3 GET, PUT, and LIST operations, as well as operations that change object tags, ACLs, or metadata, are strongly consistent. What you write is what you will read, and the results of a LIST will be an accurate reflection of what’s in the bucket.
- http://169.254.169.254/latest/meta-data/public-ipv4
  
  Instance metadata is the data about your instance that you can use to configure or manage the running instance.
  
  Instance user data is the data that you specified in the form of a configuration script while launching your instance.
  
  The following URL paths can be used to get the instance meta data and user data from within the instance: http://169.254.169.254/latest/meta-data/
  
  http://169.254.169.254/latest/user-data/
  
  Further, you can get the instance public IP via the URL - http://169.254.169.254/latest/meta-data/public-ipv4
- AWS Transit Gateway is a service that enables customers to connect their Amazon Virtual Private Clouds (VPCs) and their on-premises networks to a single gateway. With AWS Transit Gateway, you only have to create and manage a single connection from the central gateway into each Amazon VPC, on-premises data center, or remote office across your network. Transit Gateway acts as a hub that controls how traffic is routed among all the connected networks which act like spokes. So, this is a perfect use-case for the Transit Gateway
- It allows starting EC2 instances only when the IP where the call originates is within the 34.50.31.0/24 CIDR block
  
  You manage access in AWS by creating policies and attaching them to IAM identities (users, groups of users, or roles) or AWS resources. A policy is an object in AWS that, when associated with an identity or resource, defines their permissions. AWS evaluates these policies when an IAM principal (user or role) makes a request. Permissions in the policies determine whether the request is allowed or denied. Most policies are stored in AWS as JSON documents. AWS supports six types of policies: identity-based policies, resource-based policies, permissions boundaries, Organizations SCPs, ACLs, and session policies.
- Deploy the web-tier EC2 instances in two Availability Zones, behind an Elastic Load Balancer. Deploy the Amazon RDS MySQL database in Multi-AZ configuration
  
  Elastic Load Balancing automatically distributes incoming application traffic across multiple targets, such as Amazon EC2 instances, containers, IP addresses, and Lambda functions. It can handle the varying load of your application traffic in a single Availability Zone or across multiple Availability Zones. Therefore, deploying the web-tier EC2 instances in two Availability Zones, behind an Elastic Load Balancer would improve the availability of the application.
  
  Amazon RDS Multi-AZ deployments provide enhanced availability and durability for RDS database (DB) instances, making them a natural fit for production database workloads. When you provision a Multi-AZ DB Instance, Amazon RDS automatically creates a primary DB Instance and synchronously replicates the data to a standby instance in a different Availability Zone (AZ). Each AZ runs on its own physically distinct, independent infrastructure, and is engineered to be highly reliable. Deploying the Amazon RDS MySQL database in Multi-AZ configuration would improve availability and hence this is the correct option.
- Amazon S3 Glacier Deep Archive
  
  Amazon S3 Glacier and S3 Glacier Deep Archive are secure, durable, and extremely low-cost Amazon S3 cloud storage classes for data archiving and long-term backup. They are designed to deliver 99.999999999% durability, and provide comprehensive security and compliance capabilities that can help meet even the most stringent regulatory requirements.
  
  S3 Glacier Deep Archive is a new Amazon S3 storage class that provides secure and durable object storage for long-term retention of data that is accessed once or twice in a year. From just $0.00099 per GB-month (less than one-tenth of one cent, or about $1 per TB-month), S3 Glacier Deep Archive offers the lowest cost storage in the cloud, at prices significantly lower than storing and maintaining data in on-premises magnetic tape libraries or archiving data off-site.
  
  S3 Glacier Deep Archive is up to 75% less expensive than S3 Glacier and provides retrieval within 12 hours using the Standard retrieval speed. You may also reduce retrieval costs by selecting Bulk retrieval, which will return data within 48 hours.
- For each developer, define an IAM permission boundary that will restrict the managed policies they can attach to themselves
  
  AWS supports permissions boundaries for IAM entities (users or roles). A permissions boundary is an advanced feature for using a managed policy to set the maximum permissions that an identity-based policy can grant to an IAM entity. An entity's permissions boundary allows it to perform only the actions that are allowed by both its identity-based policies and its permissions boundaries. Here we have to use an IAM permission boundary. They can only be applied to roles or users, not IAM groups.
- The EBS volume was configured as the root volume of the Amazon EC2 instance. On termination of the instance, the default behavior is to also terminate the attached root volume
  
  Amazon Elastic Block Store (EBS) is an easy to use, high-performance block storage service designed for use with Amazon Elastic Compute Cloud (EC2) for both throughput and transaction-intensive workloads at any scale.
  
  When you launch an instance, the root device volume contains the image used to boot the instance. You can choose between AMIs backed by Amazon EC2 instance store and AMIs backed by Amazon EBS.
  
  By default, the root volume for an AMI backed by Amazon EBS is deleted when the instance terminates. You can change the default behavior to ensure that the volume persists after the instance terminates. Non-root EBS volumes remain available even after you terminate an instance to which the volumes were attached. Therefore, this option is correct.
- Create an auto-scaling group that spans across 2 AZ, which min=1, max=1, desired=1
  
  Amazon EC2 Auto Scaling helps you ensure that you have the correct number of Amazon EC2 instances available to handle the load for your application. You create collections of EC2 instances, called Auto Scaling groups. You can specify the minimum number of instances in each Auto Scaling group, and Amazon EC2 Auto Scaling ensures that your group never goes below this size.
  
  So we have an ASG with desired=1, across two AZ, so that if an instance goes down, it is automatically recreated in another AZ. So this option is correct.
  
  Create an Elastic IP and use the EC2 user-data script to attach it
  
  Application Load Balancer (ALB) operates at the request level (layer 7), routing traffic to targets – EC2 instances, containers, IP addresses, and Lambda functions based on the content of the request. Ideal for advanced load balancing of HTTP and HTTPS traffic, Application Load Balancer provides advanced request routing targeted at delivery of modern application architectures, including microservices and container-based applications.
  
  An Elastic IP address is a static IPv4 address designed for dynamic cloud computing. An Elastic IP address is associated with your AWS account. With an Elastic IP address, you can mask the failure of an instance or software by rapidly remapping the address to another instance in your account.
  
  Now, between the ALB and the Elastic IP. If we use an ALB, things will still work, but we will have to pay for the provisioned ALB which sends traffic to only one EC2 instance. Instead, to minimize costs, we must use an Elastic IP.
  
  Assign an EC2 Instance Role to perform the necessary API calls
  
  For that Elastic IP to be attached to our EC2 instance, we must use an EC2 user data script, and our EC2 instance must have the correct IAM permissions to perform the API call, so we need an EC2 instance role.
- Create an ASG with a launch template
  
  A launch configuration is an instance configuration template that an Auto Scaling group uses to launch EC2 instances. When you create a launch configuration, you specify information for the instances such as the ID of the Amazon Machine Image (AMI), the instance type, a key pair, one or more security groups, and a block device mapping.
  
  A launch template is similar to a launch configuration, in that it specifies instance configuration information. Included are the ID of the Amazon Machine Image (AMI), the instance type, a key pair, security groups, and the other parameters that you use to launch EC2 instances. However, defining a launch template instead of a launch configuration allows you to have multiple versions of a template.
  
  Launch Templates do support a mix of On-Demand and Spot instances, and thanks to the ASG, we get auto-scaling capabilities. Hence this is the correct option.
- Use a Web Application Firewall and setup a rate-based rule
  
  AWS WAF is a web application firewall that helps protect your web applications or APIs against common web exploits that may affect availability, compromise security, or consume excessive resources. AWS WAF gives you control over how traffic reaches your applications by enabling you to create security rules that block common attack patterns, such as SQL injection or cross-site scripting, and rules that filter out specific traffic patterns you define.
  
  The correct answer is to use WAF (which has integration on top of your ALB) and define a rate-based rule.
- Create an application that will traverse the S3 bucket, issue a Byte Range Fetch for the first 250 bytes, and store that information in RDS
  
  Amazon Simple Storage Service (Amazon S3) is an object storage service that offers industry-leading scalability, data availability, security, and performance.
  
  Using the Range HTTP header in a GET Object request, you can fetch a byte-range from an object, transferring only the specified portion. You can use concurrent connections to Amazon S3 to fetch different byte ranges from within the same object. This helps you achieve higher aggregate throughput versus a single whole-object request. Fetching smaller ranges of a large object also allows your application to improve retry times when requests are interrupted.
  
  A byte-range request is a perfect way to get the beginning of a file and ensuring we remain efficient during our scan of our S3 bucket. So this is the correct option.
- Use EC2 Instance Hibernate
  
  When you hibernate an instance, AWS signals the operating system to perform hibernation (suspend-to-disk). Hibernation saves the contents from the instance memory (RAM) to your Amazon EBS root volume. AWS then persists the instance's Amazon EBS root volume and any attached Amazon EBS data volumes.
  
  When you start your instance:
  
  The Amazon EBS root volume is restored to its previous state
  
  The RAM contents are reloaded
  
  The processes that were previously running on the instance are resumed
  
  Previously attached data volumes are reattached and the instance retains its instance ID
 