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
#### AWS Lambda Synchronous Invocations
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

