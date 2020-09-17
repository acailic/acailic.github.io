---
title: AWS Serverless Lambda
layout: post
tags: [aws, cda, serverless, lambda]
date: 2020-09-17
---

### Serverless
-	Serverless is a new paradigm in which the developers don’t have to manage servers anymore…
-	They just deploy… functions !
•	Initially... Serverless == FaaS (Function as a Service)
-	Serverless was pioneered by AWS Lambda but now also includes anything that’s managed: “databases, messaging, storage, etc.”
-	Serverless does not mean there are no servers. Tt means you just don’t manage / provision / see them
- Serverless in AWS
•	AWS Lambda
•	DynamoDB
•	AWS Cognito
•	AWS API Gateway
•	Amazon S3
•	AWS SNS & SQS
•	AWS Kinesis Data Firehose
•	Aurora Serverless
•	Step Functions
•	Fargate
### AWS Lambda
- Amazon EC2
• Virtual Servers in the Cloud
• Limited by RAM and CPU
• Continuously running 
•	Scaling means intervention to add / remove servers
- Amazon Lambda
•	Virtual functions – no servers to manage!
•	Limited by time - short executions
•	Run on-demand
•	Scaling is automated!
- Easy Pricing:
•	Pay per request and compute time
•	Free tier of 1,000,000 AWS Lambda requests and 400,000 GBs of compute time
-	Integrated with the whole AWS suite of services
-	Integrated with many programming languages. Important: Docker is not for AWS Lambda, it’s for ECS / Fargate
-	Easy monitoring through AWS CloudWatch
-	Easy to get more resources per functions (up to 3GB of RAM!)
-	Increasing RAM will also improve CPU and network!
#### AWS Lambda Pricing: example
-	You can find overall pricing information here: https://aws.amazon.com/lambda/pricing/
-	Pay per calls:
•	First 1,000,000 requests are free
•	$0.20 per 1 million requests thereafter ($0.0000002 per request)
-	Pay per duration: (in increment of 100ms)
•	400,000 GB-seconds of compute time per month if FREE
•	== 400,000 seconds if function is 1GB RAM
•	== 3,200,000 seconds if function is 128 MB RAM
•	After that $1.00 for 600,000 GB-seconds
-	It is usually very cheap to run AWS Lambda so it’s very popular
#### Lambda Synchronous Invocations
-	Synchronous: CLI, SDK, API Gateway, Application Load Balancer
•	Results is returned right away
•	Error handling must happen client side (retries, exponential backoff, etc…)
-	User Invoked:
•	Elastic Load Balancing (Application Load Balancer)
•	Amazon API Gateway
•	Amazon CloudFront (Lambda@Edge)
•	Amazon S3 Batch
-	Service Invoked:
•	Amazon Cognito
•	AWS Step Functions
-	Other Services:
•	Amazon Lex
•	Amazon Alexa
•	Amazon Kinesis Data Firehose
#### Lambda wiht ALB
-	To expose a Lambda function as an HTTP(S) endpoint…
-	You can use the Application Load Balancer (or an API Gateway)
-	The Lambda function must be registered in a target group
- ALB can support multi header  values (ALB setting). When you enable multi-value headers, HTTP headers and query string parameters that are sent with multiple values are shown as arrays within the AWS Lambda event and response objects.
#### Lambda@Edge
-  Lambda@Edge:deploy Lambda functions alongside your CloudFront CDN
•	Build more responsive applications
•	You don’t manage servers, Lambda is deployed globally
•	Customize the CDN content
•	Pay only for what you use
- globaly, with implemented filtering request
- You can use Lambda to change CloudFront requests and responses:
•	After CloudFront receives a request from a viewer (viewer request)
•	Before CloudFront forwards the request to the origin (origin request)
•	After CloudFront receives the response from the origin (origin response)
•	Before CloudFront forwards the response to the viewer (viewer response)
- You can also generate responses to viewers without ever sending the request to the origin
- Lambda@Edge: Use Cases
•	Website Security and Privacy
•	Dynamic Web Application at the Edge
•	Search Engine Optimization (SEO)
•	Intelligently Route Across Origins and Data Centers
•	Bot Mitigation at the Edge
•	Real-time Image Transformation
•	A/B Testing
•	User Authentication and Authorization
•	User Prioritization
•	User Tracking and Analytics
#### Lambda – Asynchronous Invocations
- S3, SNS, CloudWatch Events…
-	The events are placed in an Event Queue
-	Lambda attempts to retry on errors
• 3 try total,1 minute wait after 1st , then 2 minutes wait
-	Make sure the processing is idempotent (in case of retries)
- If the function is retried, you will see duplicate logs entries in CloudWatch Logs
-	Can define a DLQ (dead-letter queue) – SNS or SQS – for failed processing (need correct IAM permissions)
-	Asynchronous invocations allow you to speed up the processing if you don’t need to wait for the result (ex: you need 1000 files processed)
Lambda - Asynchronous Invocations - Services
•	Amazon Simple Storage Service (S3)
•	Amazon Simple Notification Service (SNS)
•	Amazon CloudWatch Events / EventBridge
•	AWS CodeCommit (CodeCommit Trigger: new branch, new tag, new push)
•	AWS CodePipeline (invoke a Lambda function during the pipeline, Lambda must callback)
----- other -----
•	Amazon CloudWatch Logs (log processing)
•	Amazon Simple Email Service
•	AWS CloudFormation
•	AWS Config
•	AWS IoT
•	AWS IoT Events
#### CloudWatch Events / EventBridge
#### S3 Events Notifications
#### Lambda – Event Source Mapping
#### Streams & Lambda
