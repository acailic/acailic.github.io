---
title: AWS CDA Practise test 5
layout: post
tags: [aws, cda, test]
date: 2020-08-17
---
### Monitoring
### Security
### Refactoring
- A development team uses the AWS SDK for Java to maintain an application that stores data in AWS DynamoDB. The application makes use of Scan operations to return several items from a 25 GB table. There is no possibility of creating indexes to retrieve these items predictably. Developers are trying to get these specific rows from DynamoDB as fast as possible.
Which of the following options can be used to improve the performance of the Scan operation?:Use parallel scans. 
### Development with AWS
- You are assigned as the new project lead for a web application that processes orders for customers. You want to integrate event-driven processing anytime data is modified or deleted and use a serverless approach using AWS Lambda for processing stream events.
Which of the following databases should you choose from?:Kinesis is not a database. DynamoDB
- You have uploaded a zip file to AWS Lambda that contains code files written in Node.Js. When your function is executed you receive the following output, 'Error: Memory Size: 3008 MB Max Memory Used'.
 Which of the following explains the problem? Your Lambda function was deployed with 3008MB of RAM, but it seems your code requested or used more than that, so the Lambda function failed.
- A senior cloud engineer designs and deploys online fraud detection solutions for credit card companies processing millions of transactions daily. The Elastic Beanstalk application sends files to Amazon S3 and then sends a message to an Amazon SQS queue containing the path of the uploaded file in S3. A solution architect recommended that since PUTS of existing objects in S3 deliver eventual consistency and we want to minimize the risk of consumers reading old data, it would be preferable to introduce a per-message lag so that consumers won't find a message in SQS until it has been in the queue for at least 10 seconds.
 Which SQS feature should the developer leverage?:Delay queues let you postpone the delivery of new messages to a queue for several seconds, for example, when your consumer application needs additional time to process messages. 
- You are a system administrator whose company recently moved its production application to AWS and migrated data from MySQL to AWS DynamoDB. You are adding new tables to AWS DynamoDB and need to allow your application to query your data by the primary key and an alternate key. This option must be added when first creating tables otherwise changes cannot be made afterward.
 Which of the following actions should you take?:Create a LSI. Local secondary indexes are created at the same time that you create a table. You cannot add a local secondary index to an existing table, nor can you delete any local secondary indexes that currently exist.
- DevOps engineers are developing an order processing system where notifications are sent to a department whenever an order is placed for a product. The system also pushes identical notifications of the new order to a processing module that would allow EC2 instances to handle the fulfillment of the order. In the case of processing errors, the messages should be allowed to be re-processed at a later stage and never lost.
 Which of the following solutions can be used to address this use-case?SNS + SQS.Amazon SNS enables message filtering and fanout to a large number of subscribers, including serverless functions, queues, and distributed systems. Additionally, Amazon SNS fans out notifications to end users via mobile push messages, SMS, and email.
- You have a popular three-tier web application that is used by users throughout the globe receiving thousands of incoming requests daily. You have AWS Route 53 policies to automatically distribute weighted traffic to the API resources located at URL api.global.com.
 What is an alternative way of distributing traffic to a web application?:CloudFront - Amazon CloudFront is a fast content delivery network (CDN) service that securely delivers data, videos, applications, and APIs to customers globally with low latency, high transfer speeds.
- You have an Amazon Kinesis Data Stream with 10 shards, and from the metrics, you are well below the throughput utilization of 10 MB per second to send data. You send 3 MB per second of data and yet you are receiving ProvisionedThroughputExceededException errors frequently.
 What is the likely cause of this?:The partition key that you have selected isn't distributed enough
- Your application sends messages to an Amazon Simple Queue Service (SQS) queue frequently, which are then polled by another application that specifies which message to retrieve.
  Which of the following options describe the maximum number of messages that can be retrieved at one time?: 10. After you send messages to a queue, you can receive and delete them. When you request messages from a queue, you can't specify which messages to retrieve. Instead, you specify the maximum number of messages (up to 10) that you want to retrieve.
### Deployment