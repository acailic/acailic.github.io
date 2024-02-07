---
title: AWS SAA Practise test 4
layout: post
tags: [aws, saa, test]
date: 2021-04-17
--- 


-  A limited set of core services will be replicated to the DR site ready to seamlessly take over the in the event of a disaster. All other services will be switched off. In this DR approach, you simply replicate part of your IT structure for a limited set of core services so that the AWS cloud environment seamlessly takes over in the event of a disaster.

A small part of your infrastructure is always running simultaneously syncing mutable data (as databases or documents), while other parts of your infrastructure are switched off and used only during testing.

Unlike a backup and recovery approach, you must ensure that your most critical core elements are already configured and running in AWS (the pilot light). When the time comes for recovery, you can rapidly provision a full-scale production environment around the critical core.

- Larger data migrations with AWS DMS can include many terabytes of information. This process can be cumbersome due to network bandwidth limits or just the sheer amount of data. AWS DMS can use Snowball Edge and Amazon S3 to migrate large databases more quickly than by other methods.
When you're using an Edge device, the data migration process has the following stages:
1. You use the AWS Schema Conversion Tool (AWS SCT) to extract the data locally and move it to an Edge device.
2. You ship the Edge device or devices back to AWS.
3. After AWS receives your shipment, the Edge device automatically loads its data into an Amazon S3 bucket.
4. AWS DMS takes the files and migrates the data to the target data store. If you are using change data capture (CDC), those updates are written to the Amazon S3 bucket and then applied to the target data store.


- AWS recommend that you use the AWS SDKs to make programmatic API calls to IAM. However, you can also use the IAM Query API to make direct calls to the IAM web service. An access key ID and secret access key must be used for authentication when using the Query API.

- Amazon Cognito identity pools provide temporary AWS credentials for users who are guests (unauthenticated) and for users who have been authenticated and received a token. An identity pool is a store of user identity data specific to your account.

With an identity pool, users can obtain temporary AWS credentials to access AWS services, such as Amazon S3 and DynamoDB.   

- AWS recommend that you use the AWS SDKs to make programmatic API calls to IAM. However, you can also use the IAM Query API to make direct calls to the IAM web service. An access key ID and secret access key must be used for authentication when using the Query API.


- Single sign-on using federation allows users to login to the AWS console without assigning IAM credentials. The AWS Security Token Service (STS) is a web service that enables you to request temporary, limited-privilege credentials for IAM users or for users that you authenticate (such as federated users from an on-premise directory).

Federation (typically Active Directory) uses SAML 2.0 for authentication and grants temporary access based on the users AD credentials. The user does not need to be a user in IAM.

- The aim of this solution is to create a single sign-on solution that enables users signed in to the organization’s Active Directory service to be able to connect to AWS resources. When developing a custom identity broker you use the AWS STS service. 
The AWS Security Token Service (STS) is a web service that enables you to request temporary, limited-privilege credentials for IAM users or for users that you authenticate (federated users). The steps performed by the custom identity broker to sign users into the AWS management console are:
 Verify that the user is authenticated by your local identity system.
 Call the AWS Security Token Service (AWS STS) AssumeRole or GetFederationToken API operations to obtain temporary. security credentials for the user.
 Call the AWS federation endpoint and supply the temporary security credentials to request a sign-in token.
 Construct a URL for the console that includes the token.
 Give the URL to the user or invoke the URL on the user’s behalf.


 - Launch templates enable you to store launch parameters so that you do not have to specify them every time you launch an instance. When you launch an instance using the Amazon EC2 console, an AWS SDK, or a command line tool, you can specify the launch template to use.

 - The following are a few reasons why an instance might immediately terminate:
 You’ve reached your EBS volume limit.
 An EBS snapshot is corrupt.
 The root EBS volume is encrypted and you do not have permissions to access the KMS key for decryption.
 The instance store-backed AMI that you used to launch the instance is missing a required part (an image.part.xx file).

 - A public subnet is a subnet that has an Internet Gateway attached and “Enable auto-assign public IPv4 address” enabled. Instances require a public IP or Elastic IP address. It is also necessary to have the subnet route table updated to point to the Internet Gateway and security groups and network ACLs must be configured to allow the SSH traffic on port 22.

 - The key requirements here are that you need to deploy several EC2 instances quickly to run the batch process and you must ensure that the job completes. The on-demand pricing model is the best for this ad-hoc requirement as though spot pricing may be cheaper you cannot afford to risk that the instances are terminated by AWS when the market price increases.

 - If any health check returns an unhealthy status the instance will be terminated. For the “impaired” status, the ASG will wait a few minutes to see if the instance recovers before taking action. If the “impaired” status persists, termination occurs. Unlike AZ rebalancing, termination of unhealthy instances happens first, then Auto Scaling attempts to launch new instances to replace terminated instances.

 - A tag is a label that you assign to an AWS resource. Each tag consists of a key and an optional value, both of which you define. Tags enable you to categorize your AWS resources in different ways, for example, by purpose, owner, or environment.

 - An ALB allows containers to use dynamic host port mapping so that multiple tasks from the same service are allowed on the same container host. An ALB can also route requests based on the content of the request in the host field: host-based or path-based. The NLB and CLB types of Elastic Load Balancer do not support path-based routing or host-based routing so they cannot be used for this use case.

 - CloudFront distributes traffic across multiple edge locations and filters requests to ensure that only valid HTTP(S) requests will be forwarded to backend hosts. CloudFront also supports geoblocking, which you can use to prevent requests from particular geographic locations from being served.

Auto Scaling helps to maintain a desired count of EC2 instances running at all times and setting a high maximum number of instances allows your fleet to grow and absorb some of the impact of the attack.

- You can suspend and then resume one or more of the scaling processes for your Auto Scaling group. This can be useful when you want to investigate a configuration problem or other issue with your web application and then make changes to your application, without invoking the scaling processes. You can manually move an instance from an ASG and put it in the standby state.

Instances in standby state are still managed by Auto Scaling, are charged as normal, and do not count towards available EC2 instance for workload/application use. Auto scaling does not perform health checks on instances in the standby state. Standby state can be used for performing updates/changes/troubleshooting etc. without health checks being performed or replacement instances being launched.

- Auto Scaling can perform rebalancing when it finds that the number of instances across AZs is not balanced. Auto Scaling rebalances by launching new EC2 instances in the AZs that have fewer instances first, only then will it start terminating instances in AZs that had more instances
Auto Scaling can be configured to send an SNS email when:
An instance is launched.
An instance is terminated.
An instance fails to launch.
An instance fails to terminate.

- Amazon Aurora Global Database is designed for globally distributed applications, allowing a single Amazon Aurora database to span multiple AWS regions. It replicates your data with no impact on database performance, enables fast local reads with low latency in each region, and provides disaster recovery from region-wide outages.

Aurora Global Database uses storage-based replication with typical latency of less than 1 second, using dedicated infrastructure that leaves your database fully available to serve application workloads. In the unlikely event of a regional degradation or outage, one of the secondary regions can be promoted to full read/write capabilities in less than 1 minute.

- Multi-AZ RDS creates a replica in another AZ and synchronously replicates to it (DR only).

A failover may be triggered in the following circumstances:
 Loss of primary AZ or primary DB instance failure.
 Loss of network connectivity on primary.
 Compute (EC2) unit failure on primary.
 Storage (EBS) unit failure on primary.
 The primary DB instance is changed.
 Patching of the OS on the primary DB instance.
 Manual failover (reboot with failover selected on primary).
During failover RDS automatically updates configuration (including DNS endpoint) to use the second node.

- A VPC endpoint enables you to privately connect your VPC to supported AWS services and VPC endpoint services powered by AWS PrivateLink without requiring an internet gateway, NAT device, VPN connection, or AWS Direct Connect connection.

Instances in your VPC do not require public IP addresses to communicate with resources in the service. Traffic between your VPC and the other service does not leave the Amazon network.

With a gateway endpoint you configure your route table to point to the endpoint. Amazon S3 and DynamoDB use gateway endpoints.

The table below helps you to understand the key differences between the two different types of VPC endpoint: Interface vs Gateway endpoint.
Gateway endpoint is target for specific route. Use prefix list  in route table. DynamoDB, Amazon S3, VPC endpoints Policies.
Interface Endpoint is Elastic Network Interface  with a Private IP.  Uses DNS Entries  to redirect traffic. Security Groups. API Gateway, CloudFormation, CloudWatch.

- If you are installing MySQL on an EC2 instance you cannot enable read replicas or multi-AZ. Instead you would need to use Amazon RDS with a MySQL DB engine to use these features. 
In this example a good solution is to use the native HA features of MySQL. You would want to place the second MySQL DB instance in another AZ to enable high availability and fault tolerance. 
Migrating to Amazon RDS may be a good solution but is not presented as an option.

- A CloudWatch Events rule can be used to set up automatic email notifications for Medium to High Severity findings to the email address of your choice. You simply create an Amazon SNS topic and then associate it with an Amazon CloudWatch events rule. 
Note: step by step procedures for how to set this up can be found in the article linked in the references below.

- Policies are documents that define permissions and can be applied to users, groups and roles. Policy documents are written in JSON (key value pair that consists of an attribute and a value). 
Within an IAM policy you can grant either programmatic access or AWS Management Console access to Amazon S3 resources.

- The most cost-effective solution is to first store the data in S3 Standard-IA where it will be infrequently accessed for the first three months. Then, after three months expires, transition the data to S3 Glacier where it can be stored at lower cost for the remainder of the seven year period. Expedited retrieval can bring retrieval times down to 1-5 minutes.
