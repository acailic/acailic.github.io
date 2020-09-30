---
title: AWS Serverless  Step functions and AppSync
layout: post
tags: [aws, cda, serverless, appsync, functions]
date: 2020-09-17
--- 

## AWS Step Functions
- Build serverless visual workflow to orchestrate your Lambda functions
- Represent flow as a JSON state machine
- Features: sequence, parallel, conditions, timeouts, error handling…
- Can also integrate with EC2, ECS, On premise servers, API Gateway
- Maximum execution time of 1 year
- Possibility to implement human approval feature
- Use cases:
• Order fulfillment
• Data processing
• Web applications
• Any workflow
### Step Functions – Error Handling
- Any state can encounter runtime errors for various reasons:
• State machine definition issues (for example, no matching rule in a Choice state)
• Task failures (for example, an exception in a Lambda function)
• Transient issues (for example, network partition events)
- By default, when a state reports an error, AWS Step Functions causes
the execution to fail entirely.
- Retrying failures - Retry: IntervalSeconds, MaxAttempts, BackoffRate
- Moving on - Catch: ErrorEquals, Next
- Best practice is to include data in the error messages
### AWS AppSync - Overview
- AppSync is a managed service that uses GraphQL
- GraphQL makes it easy for applications to get exactly the data they need.
- This includes combining data from one or more sources
• NoSQL data stores, Relational databases, HTTP APIs…
• Integrates with DynamoDB, Aurora, Elasticsearch & others
• Custom sources with AWS Lambda
- Retrieve data in real-time with WebSocket or MQTT on WebSocket
- For mobile apps: local data access & data synchronization
- It all starts with uploading one GraphQL schema
### AppSync – Security
- There are four ways you can authorize applications to interact with your
AWS AppSync GraphQL API:
- API_KEY
- AWS_IAM: IAM users / roles / cross-account access
- OPENID_CONNECT: OpenID Connect provider / JSON Web Token
- AMAZON_COGNITO_USER_POOLS
- For custom domain & HTTPS, use CloudFront in front of AppSync
