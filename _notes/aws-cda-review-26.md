---
title: AWS CDA Practise test 1
layout: post
tags: [aws, cda, test]
date: 2020-08-17
---
## Monitoring
- If two writes are made to a single non-versioned object at the same time, it is possible that only a single event notification will be sent - Amazon S3 event notifications are designed to be delivered at least once. Typically, event notifications are delivered in seconds but can sometimes take a minute or longer.
- Security Groups
A security group acts as a virtual firewall for your EC2 instances to control incoming and outgoing traffic. Inbound rules control the incoming traffic to your instance, and outbound rules control the outgoing traffic from your instance.
Check the security group rules of your EC2 instance. You need a security group rule that allows inbound traffic from your public IPv4 address on the proper port.
Incorrect options:
IAM Roles - Usually you run into issues with authorization of APIs with roles but not for timeout, so this option does not fit the given use-case.
The application is down - Although you can set a health check for application ping or HTTP, timeouts are usually caused by blocked firewall access.
- You terminated the container instance while it was in STOPPED state, that lead to this synchronization issues - If you terminate a container instance while it is in the STOPPED state, that container instance isn't automatically removed from the cluster. You will need to deregister your container instance in the STOPPED state by using the Amazon ECS console or AWS Command Line Interface. Once deregistered, the container instance will no longer appear as a resource in your Amazon ECS cluster.
- If you have created an organization in AWS Organizations, you can also create a trail that will log all events for all AWS accounts in that organization. This is referred to as an organization trail.
By default, CloudTrail tracks only bucket-level actions. To track object-level actions, you need to enable Amazon S3 data events - This is a correct statement. AWS CloudTrail supports Amazon S3 Data Events, apart from bucket Events. You can record all API actions on S3 Objects and receive detailed information such as the AWS account of the caller, IAM user role of the caller, time of the API call, IP address of the API, and other details. All events are delivered to an S3 bucket and CloudWatch Events, allowing you to take programmatic actions on the events.
Member accounts will be able to see the organization trail, but cannot modify or delete it - Organization trails must be created in the master account, and when specified as applying to an organization, are automatically applied to all member accounts in the organization. Member accounts will be able to see the organization trail, but cannot modify or delete it. By default, member accounts will not have access to the log files for the organization trail in the Amazon S3 bucket.
User Data is generally used to perform common automated configuration tasks and even run scripts after the instance starts. When you launch an instance in Amazon EC2, you can pass two types of user data - shell scripts and cloud-init directives. You can also pass this data into the launch wizard as plain text or as a file.
By default, scripts entered as user data are executed with root user privileges - Scripts entered as user data are executed as the root user, hence do not need the sudo command in the script. Any files you create will be owned by root; if you need non-root users to have file access, you should modify the permissions accordingly in the script.
By default, user data runs only during the boot cycle when you first launch an instance - By default, user data scripts and cloud-init directives run only during the boot cycle when you first launch an instance. You can update your configuration to ensure that your user data scripts and cloud-init directives run every time you restart your instance.
- The health check type of your instance's Auto Scaling group, must be changed from EC2 to ELB by using a configuration file - By default, the health check configuration of your Auto Scaling group is set as an EC2 type that performs a status check of EC2 instances. To automate the replacement of unhealthy EC2 instances, you must change the health check type of your instance's Auto Scaling group from EC2 to ELB by using a configuration file.
## Development with AWS
- AWS Kinesis Data Streams
Amazon Kinesis Data Streams (KDS) is a massively scalable and durable real-time data streaming service. KDS can continuously capture gigabytes of data per second from hundreds of thousands of sources such as website clickstreams, database event streams, financial transactions, social media feeds, IT logs, and location-tracking events. The data collected is available in milliseconds to enable real-time analytics use cases such as real-time dashboards, real-time anomaly detection, dynamic pricing, and more.
Amazon Kinesis Data Streams enables real-time processing of streaming big data. It provides ordering of records, as well as the ability to read and/or replay records in the same order to multiple Amazon Kinesis Applications. The Amazon Kinesis Client Library (KCL) delivers all records for a given partition key to the same record processor, making it easier to build multiple applications reading from the same Amazon Kinesis data stream (for example, to perform counting, aggregation, and filtering). Amazon Kinesis Data Streams is recommended when you need the ability for multiple applications to consume the same stream concurrently. For example, you have one application that updates a real-time dashboard and another application that archives data to Amazon Redshift. You want both applications to consume data from the same stream concurrently and independently.
KDS provides the ability for multiple applications to consume the same stream concurrently
- Correct option:!ImportValue
The intrinsic function Fn::ImportValue returns the value of an output exported by another stack. You typically use this function to create cross-stack references.
Incorrect options:
!Ref - Returns the value of the specified parameter or resource.
!GetAtt - Returns the value of an attribute from a resource in the template.
!Sub - Substitutes variables in an input string with values that you specify.
- The Amazon Simple Workflow Service (Amazon SWF) makes it easy to build applications that coordinate work across distributed components. In Amazon SWF, a task represents a logical unit of work that is performed by a component of your application. Coordinating tasks across the application involves managing intertask dependencies, scheduling, and concurrency per the logical flow of the application. Amazon SWF gives you full control over implementing tasks and coordinating them without worrying about underlying complexities such as tracking their progress and maintaining their state. SWF ensures the task is assigned only once while SQS may deliver the message multiple times. SWF has task-oriented APIs and SQS has message-oriented APIs.
- Amazon ElastiCache allows you to seamlessly set up, run, and scale popular open-Source compatible in-memory data stores in the cloud. Build data-intensive apps or boost the performance of your existing databases by retrieving data from high throughput and low latency in-memory data stores. Amazon ElastiCache is a popular choice for real-time use cases like Caching, Session Stores, Gaming, Geospatial Services, Real-Time Analytics, and Queuing.
Broadly, you can set up two types of caching strategies:
Lazy Loading.
Write-Through.
Use a caching strategy to write to the backend first and then invalidate the cache
This option is similar to the write-through strategy wherein the application writes to the backend first and then invalidate the cache. As the cache gets invalidated, the caching engine would then fetch the latest value from the backend, thereby making sure that the product prices and product description stay consistent with the backend.
- Create a saved configuration in Team A's account and download it to your local machine. Make the account-specific parameter changes and upload to the S3 bucket in Team B's account. From Elastic Beanstalk console, create an application from 'Saved Configurations - You must use saved configurations to migrate an Elastic Beanstalk environment between AWS accounts. You can save your environment's configuration as an object in Amazon Simple Storage Service (Amazon S3) that can be applied to other environments during environment creation, or applied to a running environment. Saved configurations are YAML formatted templates that define an environment's platform version, tier, configuration option settings, and tags. Download the saved configuration to your local machine. Change your account-specific parameters in the downloaded configuration file, and then save the changes. For example, change the key pair name, subnet ID, or application name (such as application-b-name). Upload the saved configuration from your local machine to an S3 bucket in Team B's account. From this account, create a new Beanstalk application by choosing 'Saved Configurations' from the navigation panel.
- SNS + SQS. Amazon SNS enables message filtering and fanout to a large number of subscribers, including serverless functions, queues, and distributed systems. Additionally, Amazon SNS fans out notifications to end users via mobile push messages, SMS, and email.Amazon Simple Queue Service (SQS) is a fully managed message queuing service that enables you to decouple and scale microservices, distributed systems, and serverless applications. SQS offers two types of message queues. Standard queues offer maximum throughput, best-effort ordering, and at-least-once delivery. SQS FIFO queues are designed to guarantee that messages are processed exactly once, in the exact order that they are sent.

SNS and SQS can be used to create a fanout messaging scenario in which messages are "pushed" to multiple subscribers, which eliminates the need to periodically check or poll for updates and enables parallel asynchronous processing of the message by the subscribers. SQS can allow for later re-processing and dead letter queues. This is called the fan-out pattern.

- ALB + ECS

Amazon Elastic Container Service (ECS) is a highly scalable, high-performance container management service that supports Docker containers and allows you to easily run applications on a managed cluster of Amazon EC2 instances.
Elastic Load Balancing automatically distributes incoming application traffic across multiple targets, such as Amazon EC2 instances, containers, IP addresses, and Lambda functions. It can handle the varying load of your application traffic in a single Availability Zone or across multiple Availability Zones.

When you use ECS with a load balancer such as ALB deployed across multiple Availability Zones, it helps provide a scalable and highly available REST API.

API Gateway + Lambda

Amazon API Gateway is a fully managed service that makes it easy for developers to publish, maintain, monitor, and secure APIs at any scale. Using API Gateway, you can create an API that acts as a “front door” for applications to access data, business logic, or functionality from your back-end services, such as EC2 or Lambda functions.

- Define a dev environment with a single instance and a 'load test' environment that has settings close to production environment

AWS Elastic Beanstalk makes it easy to create new environments for your application. You can create and manage separate environments for development, testing, and production use, and you can deploy any version of your application to any environment. Environments can be long-running or temporary. When you terminate an environment, you can save its configuration to recreate it later.

It is common practice to have many environments for the same application. You can deploy multiple environments when you need to run multiple versions of an application. So for the given use-case, you can set up 'dev' and 'load test' environment.
- Amazon EC2 Reserved Instances - Reserved instances can provide a capacity reservation, offering additional confidence in your ability to launch the number of instances you have reserved when you need them. You save money going with Reserved instances vs on-demand especially in a year's worth of time.

Reserved Instances are not physical instances, but rather a billing discount applied to the use of On-Demand Instances in your account. These On-Demand Instances must match certain attributes, such as instance type and Region, to benefit from the billing discount. So, there is no performance difference between an On-Demand instance or a Reserved instance.Amazon EC2 Spot Instances - A Spot Instance is an unused EC2 instance that is available for less than the On-Demand price. Because Spot Instances enable you to request unused EC2 instances at steep discounts, you can lower your Amazon EC2 costs significantly. Spot instances are useful if your applications can be interrupted, like data analysis, batch jobs, background processing, and optional tasks. Spot instances can be pulled down anytime without prior notice. Hence, not the right choice for the current scenario.

Amazon EC2 On-Demand Instances - With On-Demand Instances, you pay for compute capacity by the second with no long-term commitments. You have full control over its lifecycle—you decide when to launch, stop, hibernate, start, reboot, or terminate it. But, On-Demand instances cost a lot more than Reserved instances. Here, in our use case, we already know that the systems are required for a complete year, so making use of Reserved Instances discount makes a lot more sense.

On-premise EC2 instance - On-premise implies the client has to maintain the physical machines, their capacity provisioning and maintenance. Not an option when the client is planning to move to AWS Cloud.
- Specify a ProjectionExpression: A projection expression is a string that identifies the attributes you want. To retrieve a single attribute, specify its name. For multiple attributes, the names must be comma-separated.
- Create an IAM role in account B with access to DynamoDB. Modify the trust policy of the role in Account B to allow the execution role of Lambda to assume this role. Update the Lambda function code to add the AssumeRole API call

You can give a Lambda function created in one account ("account A") permissions to assume a role from another account ("account B") to access resources such as DynamoDB or S3 bucket. You need to create an execution role in Account A that gives the Lambda function permission to do its work. Then you need to create a role in account B that the Lambda function in account A assumes to gain access to the cross-account DynamoDB table. Make sure that you modify the trust policy of the role in Account B to allow the execution role of Lambda to assume this role. Finally, update the Lambda function code to add the AssumeRole API call.
- Set up reserved concurrency for the Lambda function B so that it throttles if it goes above a certain concurrency limit

Concurrency is the number of requests that a Lambda function is serving at any given time. If a Lambda function is invoked again while a request is still being processed, another instance is allocated, which increases the function's concurrency.

To ensure that a function can always reach a certain level of concurrency, you can configure the function with reserved concurrency. When a function has reserved concurrency, no other function can use that concurrency. More importantly, reserved concurrency also limits the maximum concurrency for the function, and applies to the function as a whole, including versions and aliases. Therefore using reserved concurrency for Lambda function B would limit its maximum concurrency and allow Lambda function A to execute without getting throttled.
- Increase the RAM assigned to your Lambda function

AWS Lambda lets you run code without provisioning or managing servers. You pay only for the compute time you consume.

In the AWS Lambda resource model, you choose the amount of memory you want for your function which allocates proportional CPU power and other resources. This means you will have access to more compute power when you choose one of the new larger settings. You can set your memory in 64MB increments from 128MB to 3008MB. You access these settings when you create a function or update its configuration. The settings are available using the AWS Management Console, AWS CLI, or SDKs. Therefore, by increasing the amount of memory available to the Lambda functions, you can run the compute-heavy workflows.

-  !Ref
The intrinsic function Ref returns the value of the specified parameter or resource. When you specify a parameter's logical name, it returns the value of the parameter, when you specify a resource's logical name, it returns a value that you can typically use to refer to that resource such as a physical ID. Take a look at this YAML sample template:
MyEIP:
  Type: "AWS::EC2::EIP"
  Properties:
    InstanceId: !Ref MyEC2Instance
Incorrect options:

!GetAtt - This function returns the value of an attribute from a resource in the template. The YAML syntax is like so:

!GetAtt logicalNameOfResource.attributeName

!Param - This is not a valid function name. This option has been added as a distractor.

!Join - This function appends a set of values into a single value, separated by the specified delimiter. The YAML syntax is like so:

!Join [ delimiter, [ comma-delimited list of values ] ]

