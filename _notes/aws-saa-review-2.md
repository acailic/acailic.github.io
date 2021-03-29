---
title: AWS SAA Practise test 2
layout: post
tags: [aws, saa, test]
date: 2020-08-17
---  

-  you can't modify an existing unencrypted Amazon RDS DB instance to make the instance encrypted, and you can't create an encrypted read replica from an unencrypted instance.
 However, you can use the Amazon RDS snapshot feature to encrypt an unencrypted snapshot that's taken from the RDS database that you want to encrypt. Restore a new RDS DB instance from the encrypted snapshot to deploy a new encrypted DB instance. Finally, switch your connections to the new DB instance.
- Though this sounds like a good use case for scheduled actions, both answers using scheduled actions will have 20 instances running regardless of actual demand. A better option to be more cost effective is to use a target tracking action that triggers at a lower CPU threshold.
  
  With this solution the scaling will occur before the CPU utilization gets to a point where performance is affected. This will result in resolving the performance issues whilst minimizing costs. Using a reduced cooldown period will also more quickly terminate unneeded instances, further reducing costs.
- As the data is stored both in the EBS volumes (temporarily) and the RDS database, both the EBS and RDS volumes must be encrypted at rest. This can be achieved by enabling encryption at creation time of the volume and AWS KMS keys can be used to encrypt the data. This solution meets all requirements.
- The solution must use NFS file shares to access the migrated data without code modification. This means you can use either Amazon EFS or AWS Storage Gateway – File Gateway. Both of these can be mounted using NFS from on-premises applications.
  
  However, EFS is the wrong answer as the solution asks to maximize availability and durability. The File Gateway backs off of Amazon S3 which has much higher availability and durability than EFS which is why it is the best solution for this scenario.
- A presigned URL gives you access to the object identified in the URL. When you create a presigned URL, you must provide your security credentials and then specify a bucket name, an object key, an HTTP method (PUT for uploading objects), and an expiration date and time. The presigned URLs are valid only for the specified duration. That is, you must start the action before the expiration date and time.
  
  This is the most secure way to provide the vendor with time-limited access to the log file in the S3 bucket.
- RedShift is a columnar data warehouse DB that is ideal for running long complex queries. RedShift can also improve performance for repeat queries by caching the result and returning the cached result when queries are re-run. Dashboard, visualization, and business intelligence (BI) tools that execute repeat queries see a significant boost in performance due to result caching.
- To prevent users from circumventing the controls implemented on CloudFront (using WAF or presigned URLs / signed cookies) you can use an origin access identity (OAI). An OAI is a special CloudFront user that you associate with a distribution.
  
  The next step is to change the permissions either on your Amazon S3 bucket or on the files in your bucket so that only the origin access identity has read permission (or read and download permission). This can be implemented through a bucket policy.
  
  To control access at the CloudFront layer the AWS Web Application Firewall (WAF) can be used. With WAF you must create an ACL that includes the IP restrictions required and then associate the web ACL with the CloudFront distribution.
- The transition should be to Standard-IA rather than One Zone-IA. Though One Zone-IA would be cheaper, it also offers lower availability and the question states the objects “must remain immediately available”. Therefore the availability is a consideration.
  
  Though there is no minimum duration when storing data in S3 Standard, you cannot transition to Standard IA within 30 days.
- The data is already on an SSD-backed volume (gp2), therefore to improve performance the best option is to migrate the data onto a provisioned IOPS SSD (io1) volume type which will provide improved I/O performance and therefore reduce wait times.
- AWS Direct Connect provides a secure, reliable and private connection. However, lead times are often longer than 1 month so it cannot be used to migrate data within the timeframes. Therefore, it is better to use AWS Snowball to move the data and order a Direct Connect connection to satisfy the other requirement later on. In the meantime the organization can use an AWS VPN for secure, private access to their VPC.
- The requirement is that the objects must be encrypted before they are uploaded. The only option presented that meets this requirement is to use client-side encryption. You then have two options for the keys you use to perform the encryption:
  
  • Use a customer master key (CMK) stored in AWS Key Management Service (AWS KMS).
  
  • Use a master key that you store within your application.
  
  In this case the correct answer is to use an AWS KMS key. Note that you cannot use client-side encryption with keys managed by Amazon S3.
- All solutions presented are highly available. The key requirement that must be satisfied is that the solution should be cost-effective and you must choose the most cost-effective option.
  
  Therefore, it’s best to eliminate services such as Amazon EC2 and ELB as these require ongoing costs even when they’re not used. Instead, a fully serverless solution should be used. AWS Lambda, Amazon S3 and CloudFront are the best services to use for these requirements.
- Amazon FSx for Lustre is ideal for high performance computing (HPC) workloads running on Linux instances. FSx for Lustre provides a native file system interface and works as any file system does with your Linux operating system.
  
  When linked to an Amazon S3 bucket, FSx for Lustre transparently presents objects as files, allowing you to run your workload without managing data transfer from S3.
  
  This solution provides all requirements as it enables Linux workloads to use the native file system interfaces and to use S3 for long-term and cost-effective storage of output files.
- In this scenario we need to keep the objects in the STANDARD storage class for 30 days as the objects are being frequently accessed. We can configure a lifecycle action that then transitions the objects to INTELLIGENT_TIERING, STANDARD_IA, or ONEZONE_IA. After that we don’t need the objects so they can be expired.
  
  All other options do not meet the stated requirements or are not supported lifecycle transitions. For example:
  
  · You cannot transition to REDUCED_REDUNDANCY from any storage class.
  
  · Transitioning from STANDARD_IA to ONEZONE_IA is possible but we do not want to keep the objects so it incurs unnecessary costs.
  
  · Transitioning to GLACIER is possible but again incurs unnecessary costs.
- In this scenario an inbound rule is required to allow traffic from any internet client to the web front end on SSL/TLS port 443. The source should therefore be set to 0.0.0.0/0 to allow any inbound traffic.
  
  To secure the connection from the web frontend to the database tier, an outbound rule should be created from the public EC2 security group with a destination of the private EC2 security group. The port should be set to 1433 for MySQL. The private EC2 security group will also need to allow inbound traffic on 1433 from the public EC2 security group.
- Amazon RDS is a managed service and you do not need to manage the instances. This is an ideal backend for the application and you can run a MySQL database on RDS without any refactoring. For the application components these can run on Docker containers with AWS Fargate. Fargate is a serverless service for running containers on AWS.
- Server-side encryption is about protecting data at rest. Server-side encryption encrypts only the object data, not object metadata. Using server-side encryption with customer-provided encryption keys (SSE-C) allows you to set your own encryption keys. With the encryption key you provide as part of your request, Amazon S3 manages the encryption as it writes to disks and decryption when you access your objects. Therefore, you don't need to maintain any code to perform data encryption and decryption. The only thing you do is manage the encryption keys you provide.
  
  When you upload an object, Amazon S3 uses the encryption key you provide to apply AES-256 encryption to your data and removes the encryption key from memory. When you retrieve an object, you must provide the same encryption key as part of your request. Amazon S3 first verifies that the encryption key you provided matches and then decrypts the object before returning the object data to you.
- Private access to public services such as Amazon S3 can be achieved by creating a VPC endpoint in the VPC. For S3 this would be a gateway endpoint. The bucket policy can then be configured to restrict access to the S3 endpoint only which will ensure that only services originating from the VPC will be granted access.
- The ELB Application Load Balancer can route traffic based on data included in the request including the host name portion of the URL as well as the path in the URL. Creating a rule to route traffic based on information in the path will work for this solution and ALB works well with Amazon ECS.
  
  The diagram below depicts a configuration where a listener directs traffic that comes in with /orders in the URL to the second target group and all other traffic to the first target group:
- An instance store provides temporary block-level storage for your instance. This storage is located on disks that are physically attached to the host computer. Instance store is ideal for temporary storage of information that changes frequently, such as buffers, caches, scratch data, and other temporary content, or for data that is replicated across a fleet of instances, such as a load-balanced pool of web servers. Some instance types use NVMe or SATA-based solid state drives (SSD) to deliver high random I/O performance. This is a good option when you need storage with very low latency, but you don't need the data to persist when the instance terminates or you can take advantage of fault-tolerant architectures.
                                                                                                                                                                                                                                                                                                                                                                                                                                     
                                                                                                                                                                                                                                                                                                                                                                                                                                     In this scenario the data is replicated and fault tolerant so the best option to provide the level of performance required is to use instance store volumes.

- The correct solution is to use AWS PrivateLink in a service provider model. In this configuration a network load balancer will be implemented in the service provider VPC (the one with the ECS cluster in this example), and a PrivateLink endpoint will be created in the consumer VPC (the one with the company’s application). 
- The permissions boundary for an IAM entity (user or role) sets the maximum permissions that the entity can have. This can change the effective permissions for that user or role. The effective permissions for an entity are the permissions that are granted by all the policies that affect the user or role. Within an account, the permissions for an entity can be affected by identity-based policies, resource-based policies, permissions boundaries, Organizations SCPs, or session policies.
- AWS Global Accelerator is a service that improves the availability and performance of applications with local or global users. You can configure the ALB as a target and Global Accelerator will automatically route users to the closest point of presence.
  
  Failover is automatic and does not rely on any client side cache changes as the IP addresses for Global Accelerator are static anycast addresses. Global Accelerator also uses the AWS global network which ensures consistent performance.
- VPCs can be shared among multiple AWS accounts. Resources can then be shared amongst those accounts. However, to restrict access so that consumers cannot connect to other instances in the VPC the best solution is to use PrivateLink to create an endpoint for the application. The endpoint type will be an interface endpoint and it uses an NLB in the shared services VPC.
- IAM roles should be used in place of storing credentials on Amazon EC2 instances. This is the most secure way to provide permissions to EC2 as no credentials are stored and short-lived credentials are obtained using AWS STS. Additionally, the policy attached to the role should provide least privilege permissions.
- ElastiCache can be deployed in the U.S east region to provide high-speed access to the content. ElastiCache Redis has a good use case for autocompletion (see links below).
- The performance issues can be easily resolved by offloading the SQL queries the business analysts are performing to a read replica. This ensures that data that is being queries is up to date and the existing web application does not require any modifications to take place.
- You can passthrough encrypted traffic with an NLB and terminate the SSL on the EC2 instances, so this is a valid answer.
  
  You can use a HTTPS listener with an ALB and install certificates on both the ALB and EC2 instances. This does not use passthrough, instead it will terminate the first SSL connection on the ALB and then re-encrypt the traffic and connect to the EC2 instances.
- With IAM roles for Amazon ECS tasks, you can specify an IAM role that can be used by the containers in a task. Applications must sign their AWS API requests with AWS credentials, and this feature provides a strategy for managing credentials for your applications to use, similar to the way that Amazon EC2 instance profiles provide credentials to EC2 instances.
  
  You define the IAM role to use in your task definitions, or you can use a taskRoleArn override when running a task manually with the RunTask API operation.
  
  Note that there are instances roles and task roles that you can assign in ECS when using the EC2 launch type. The task role is better when you need to assign permissions for just that specific task: