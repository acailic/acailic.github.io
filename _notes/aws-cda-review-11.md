---
title: AWS Serverless Monitoring & Audit
layout: post
tags: [aws, cda, monitoring, audit]
date: 2020-09-18
---
#### AWS Serverless Monitoring & Audit


### Questions
- We'd like to have CloudWatch Metrics for EC2 at a 1 minute rate. What should we do?:Enable Detailed Monitoring.
- High Resolution Custom Metrics can have a minimum resolution of: 1 sec
- To send a custom metric to CloudWatch, which API should we use?:PutMetricData
- Your CloudWatch alarm is triggered and controls an ASG. The alarm should trigger 1 instance being deleted from your ASG, but your ASG has already 2 instances running and the minimum capacity is 2. What will happen?:The alarm will remain in "ALARM" state but never decrease the number of instances in my ASG
- An Alarm on a High Resolution Metric can be triggered as often as: 10s
- CloudWatch logs automatically expire after 7 days by default?:They never expire by default.No.
- CloudWatch Logs expiration policy should be defined at which level?:LogGroups
- My application traces appear in X-Ray when I run the application on my local laptop. When I deploy my application to my Elastic Beanstalk, the traces do not appear in X-Ray. Why?:A config file is missing in .ebextensions. folder of your code
- My application traces appear in X-Ray when I run the application on my local laptop. When I deploy my application to my EC2 instances with CodeDeploy, the traces do not appear in X-Ray. Why?:The X-Ray daemon is not running on the EC2 instance 
- The X-Ray daemon is running on my EC2, and my application manages to send X-Ray traces from my computer, but it still doesn't work from my EC2 instance. What's wrong?:Your IAM role for your EC2 instance doesn't have the required permissions to send data to X-Ray
- All of a sudden, your CodePipeline breaks because it says it cannot find the target Elastic Beanstalk environment to deploy your application to. What should you do to find the root cause of this problem?:Look in CloudTrail for a "delete" event in Elastic Beanstalk
- How should you configure the XRay daemon to send traces across accounts?:Create a role on another account, and allow a role in your account to assume that role.
- You would like to index your XRay traces in order to search and filter through them efficiently. What should you use?: Annotations
- You would like to use a service that would enable you to get cross-account tracing and visualization. Which service do you recommend?:AWS X-Ray. ???
- Which API is NOT used for writing to X-Ray?:BatchGetTraces 
- 
