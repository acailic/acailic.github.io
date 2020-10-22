---
title: AWS CDA Practise test 4
layout: post
tags: [aws, cda, test]
date: 2020-08-17
---
### Monitoring
- A telecommunications company that provides internet service for mobile device users maintains over 100 c4.large instances in the us-east-1 region. The EC2 instances run complex algorithms. The manager would like to track CPU utilization of the EC2 instances as frequently as every 10 seconds.
Which of the following represents the BEST solution for the given use-case?:Create a high-resolution custom metric and push the data using a script triggered every 10 seconds
- A startup manages its Cloud resources with Elastic Beanstalk. The environment consists of few Amazon EC2 instances, an Auto Scaling Group (ASG), and an Elastic Load Balancer. Even after the Load Balancer marked an EC2 instance as unhealthy, the ASG has not replaced it with a healthy instance.
As a Developer, suggest the necessary configurations to automate the replacement of unhealthy instance:The health check type of your instance's Auto Scaling group, must be changed from EC2 to ELB by using a configuration file
- An IT company has migrated to a serverless application stack on the AWS Cloud with the compute layer being implemented via Lambda functions. The engineering managers would like to actively troubleshoot any failures in the Lambda functions.
  As a Developer Associate, which of the following solutions would you suggest for this use-case?The developers should insert logging statements in the Lambda function code which are then available via CloudWatch logs
- A developer from your team has configured the load balancer to route traffic equally between instances or across Availability Zones. However, Elastic Load Balancing (ELB) routes more traffic to one instance or Availability Zone than the others.
  Why is this happening and how can it be fixed? (Select two) Instances of a specific capacity type aren’t equally distributed across Availability Zones. Sticky sessions are enabled for the load balancer.
- You are working for a shipping company that is automating the creation of ECS clusters with an Auto Scaling Group using an AWS CloudFormation template that accepts cluster name as its parameters. Initially, you launch the template with input value 'MainCluster', which deployed five instances across two availability zones. The second time, you launch the template with an input value 'SecondCluster'. However, the instances created in the second run were also launched in 'MainCluster' even after specifying a different cluster name. What is the root cause of this issue?The cluster name Parameter has not been updated in the file /etc/ecs/ecs.config during bootstrap.
- You company runs business logic on smaller software components that perform various functions. Some functions process information in a few seconds while others seem to take a long time to complete. Your manager asked you to decouple components that take a long time to ensure software applications stay responsive under load. You decide to configure Amazon Simple Queue Service (SQS) to work with your Elastic Beanstalk configuration.
Which of the following Elastic Beanstalk environment should you choose to meet this requirement?Dedicated worker environment
- As a Senior Developer, you manage 10 Amazon EC2 instances that make read-heavy database requests to the Amazon RDS for PostgreSQL. You need to make this architecture resilient for disaster recovery. Which of the following features will help you prepare for database disaster recovery? (Select two) Enable the automated backup feature of Amazon RDS in a multi-AZ deployment that creates backups in a single AWS Region.Use cross-Region Read Replicas
- An organization with online transaction processing (OLTP) workloads have successfully moved to DynamoDB after having many issues with traditional database systems. However, a few months into production, DynamoDB tables are consistently recording high latency.
 As a Developer Associate, which of the following would you suggest to reduce the latency? (Select two) Use eventually consistent reads in place of strongly consistent reads whenever possible. Consider using Global tables if your application is accessed by globally distributed users
 - Your team lead has requested code review of your code for Lambda functions. Your code is written in Python and makes use of the Amazon Simple Storage Service (S3) to upload logs to an S3 bucket. After the review, your team lead has recommended reuse of execution context to improve the Lambda performance.
 Which of the following actions will help you implement the recommendation?Move the Amazon S3 client initialization, out of your function handler
 - You have deployed a traditional 3-tier web application architecture with a Classic Load Balancer, an Auto Scaling group, and an Amazon Relational Database Service (RDS) database. Users are reporting that they have to re-authenticate into the website often.
 What should you do to make the application tier stateless and outsource the session information?Add an ElastiCache Cluster.
 
 ### Deployment
 - AWS CloudFormation helps model and provision all the cloud infrastructure resources needed for your business.
 Which of the following services rely on CloudFormation to provision resources (Select two)?AWS Elastic Beanstalk.AWS Serverless Application Model (AWS SAM).
 
 ### Security
 ### Refactoring
 ### Development with AWS