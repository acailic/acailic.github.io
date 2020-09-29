---
title: AWS Serverless SAM
layout: post
tags: [aws, cda, serverless, sam]
date: 2020-09-17
---
### AWS SAM
- SAM = Serverless Application Model
- Framework for developing and deploying serverless applications
- All the configuration is YAML code
- Generate complex CloudFormation from simple SAM YAML file
- Supports anything from CloudFormation: Outputs, Mappings,
Parameters, Resources…
- Only two commands to deploy to AWS
- SAM can use CodeDeploy to deploy Lambda functions
- SAM can help you to run Lambda, API Gateway, DynamoDB locally

#### AWS SAM – Recipe
- Transform Header indicates it’s SAM template:
• Transform: 'AWS::Serverless-2016-10-31'
- Write Code
• AWS::Serverless::Function
• AWS::Serverless::Api
• AWS::Serverless::SimpleTable
- Package & Deploy:
• aws cloudformation package / sam package
• aws cloudformation deploy / sam deploy

#### Deep dive into SAM deployment
- aws cloudformation package
• SAM Template YAML file trasnform into 
• app code +swagger zip and upload to code S3 bucket
- aws cloudformation deploy
• Generated Template CloudFormation YAML
• Create and execute change set to stack

#### SAM Policy Templates
- List of templates to apply permissions to
your Lambda Functions
- Full list available here:
https://docs.aws.amazon.com/serverlessapplicationmodel/
latest/developerguide/serverlesspolicy-
templates.html#serverless-policytemplate-
table
- Important examples:
• S3ReadPolicy: Gives read only permissions to
objects in S3
• SQSPollerPolicy: Allows to poll an SQS queue
• DynamoDBCrudPolicy: CRUD = create read
update delete

###  SAM and CodeDeploy
- SAM framework natively uses
CodeDeploy to update Lambda
functions
- Traffic Shifting feature
- Pre and Post traffic hooks
features to validate deployment
(before the traffic shift starts and
after it ends)
- Easy & automated rollback using
CloudWatch Alarm

### SAM – Exam Summary
- SAM is built on CloudFormation
- SAM requires the Transform and Resources sections
- Commands to know:
• sam build: fetch dependencies and create local deployment artifacts
• sam package: package and upload to Amazon S3, generate CF template
• sam deploy: deploy to CloudFormation
- SAM Policy templates for easy IAM policy definition
- SAM is integrated with CodeDeploy to do deploy to Lambda aliases

### Questions
-
