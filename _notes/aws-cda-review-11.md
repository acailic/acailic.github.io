---
title: AWS Monitoring & Audit
layout: post
tags: [aws, cda, monitoring, audit]
date: 2020-09-18
---
#### AWS Monitoring & Audit
### Monitoring in AWS
- AWS CloudWatch:
• Metrics: Collect and track key metrics
• Logs: Collect, monitor, analyze and store log files
• Events: Send notifications when certain events happen in your AWS
• Alarms: React in real-time to metrics / events
- AWS X-Ray:
• Troubleshooting application performance and errors
• Distributed tracing of microservices
- AWS CloudTrail:
• Internal monitoring of API calls being made
• Audit changes to AWS Resources by your users
### AWS CloudWatch Metrics
- CloudWatch provides metrics for every services in AWS
- Metric is a variable to monitor (CPUUtilization, NetworkIn…)
- Metrics belong to namespaces
- Dimension is an attribute of a metric (instance id, environment, etc…).
- Up to 10 dimensions per metric
- Metrics have timestamps
- Can create CloudWatch dashboards of metrics
#### AWS CloudWatch EC2 Detailed monitoring
- EC2 instance metrics have metrics “every 5 minutes”
- With detailed monitoring (for a cost), you get data “every 1 minute”
- Use detailed monitoring if you want to more prompt scale your ASG!
- The AWS Free Tier allows us to have 10 detailed monitoring metrics
- Note: EC2 Memory usage is by default not pushed (must be pushed
from inside the instance as a custom metric)
#### AWS CloudWatch Custom Metrics
- Possibility to define and send your own custom metrics to CloudWatch
- Ability to use dimensions (attributes) to segment metrics
• Instance.id
• Environment.name
- Metric resolution (StorageResolution API parameter – two possible value):
• Standard: 1 minute (60 seconds)
• High Resolution: 1 second – Higher cost
- Use API call PutMetricData
- Use exponential back off in case of throttle errors
#### AWS CloudWatch Alarms
- Alarms are used to trigger notifications for any metric
- Alarms can go to Auto Scaling, EC2 Actions, SNS notifications
- Various options (sampling, %, max, min, etc…)
- Alarm States:
• OK
• INSUFFICIENT_DATA
• ALARM
- Period:
• Length of time in seconds to evaluate the metric
• High resolution custom metrics: can only choose 10 sec or 30 sec
#### AWS CloudWatch Logs
- Applications can send logs to CloudWatch using the SDK
- CloudWatch can collect log from:
• Elastic Beanstalk: collection of logs from application
• ECS: collection from containers
• AWS Lambda: collection from function logs
• VPC Flow Logs: VPC specific logs
• API Gateway
• CloudTrail based on filter
• CloudWatch log agents: for example on EC2 machines
• Route53: Log DNS queries
- CloudWatch Logs can go to:
• Batch exporter to S3 for archival
• Stream to ElasticSearch cluster for further analytics
- CloudWatch Logs can use filter expressions
- Logs storage architecture:
• Log groups: arbitrary name, usually representing an application
• Log stream: instances within application / log files / containers
- Can define log expiration policies (never expire, 30 days, etc..)
- Using the AWS CLI we can tail CloudWatch logs
- To send logs to CloudWatch, make sure IAM permissions are correct!
- Security: encryption of logs using KMS at the Group Level 
#### CloudWatch Logs for EC2
- By default, no logs from your EC2
machine will go to CloudWatch
- You need to run a CloudWatch
agent on EC2 to push the log files
you want
- Make sure IAM permissions are
correct
- The CloudWatch log agent can be
setup on-premises too
#### CloudWatch Logs Agent & Unified Agent
- For virtual servers (EC2 instances, on-premise servers…)
- CloudWatch Logs Agent
• Old version of the agent
• Can only send to CloudWatch Logs
- CloudWatch Unified Agent
• Collect additional system-level metrics such as RAM, processes, etc…
• Collect logs to send to CloudWatch Logs
• Centralized configuration using SSM Parameter Store
#### CloudWatch Unified Agent – Metrics
- Collected directly on your Linux server / EC2 instance
- CPU (active, guest, idle, system, user, steal)
- Disk metrics (free, used, total), Disk IO (writes, reads, bytes, iops)
- RAM (free, inactive, used, total, cached)
- Netstat (number ofTCP and UDP connections, net packets, bytes)
- Processes (total, dead, bloqued, idle, running, sleep)
- Swap Space (free, used, used %)
- Reminder: out-of-the box metrics for EC2 – disk, CPU, network (high level)
#### CloudWatch Logs Metric Filter
- CloudWatch Logs can use filter expressions
• For example, find a specific IP inside of a log
• Or count occurrences of “ERROR” in your logs
• Metric filters can be used to trigger alarms
- Filters do not retroactively filter data. Filters only publish the metric data
points for events that happen after the filter was created.
#### AWS CloudWatch Events
- Schedule: Cron jobs
- Event Pattern: Event rules to react to a service doing something
- Ex: CodePipeline state changes!
- Triggers to Lambda functions, SQS/SNS/Kinesis Messages
- CloudWatch Event creates a small JSON document to give information
about the change
### Amazon EventBridge
- EventBridge is the next evolution of CloudWatch Events
- Default event bus: generated by AWS services (CloudWatch Events)
- Partner event bus: receive events from SaaS service or applications
(Zendesk, DataDog, Segment, Auth0…)
- Custom Event buses: for your own applications
- Event buses can be accessed by other AWS accounts
- Rules: how to process the events (similar to CloudWatch Events)
#### Amazon EventBridge Schema Registry
- EventBridge can analyze the events in
your bus and infer the schema
- The Schema Registry allows you to
generate code for your application, that
will know in advance how data is
structured in the event bus
- Schema can be versioned
### Amazon EventBridge vs CloudWatch Events
- Amazon EventBridge builds upon and extends CloudWatch Events.
- It uses the same service API and endpoint, and the same underlying service
infrastructure.
- EventBridge allows extension to add event buses for your custom
applications and your third-party SaaS apps.
- Event Bridge has the Schema Registry capability
- EventBridge has a different name to mark the new capabilities
- Over time, the CloudWatch Events name will be replaced with EventBridge
### AWS X-Ray
- Debugging in Production, the good old way:
• Test locally
• Add log statements everywhere
• Re-deploy in production
- Log formats differ across applications using CloudWatch and analytics is
hard.
- Debugging: monolith “easy”, distributed services “hard”
- No common views of your entire architecture!
- Enter… AWS X-Ray!
#### AWS X-Ray advantages
- Troubleshooting performance (bottlenecks)
- Understand dependencies in a microservice architecture
- Pinpoint service issues
- Review request behavior
- Find errors and exceptions
- Are we meeting time SLA?
- Where I am throttled?
- Identify users that are impacted
#### X-Ray compatibility
- AWS Lambda
- Elastic Beanstalk
- ECS
- ELB
- API Gateway
- EC2 Instances or any application server (even on premise)
#### AWS X-Ray Leverages Tracing
- Tracing is an end to end way to following a “request”
- Each component dealing with the request adds its own “trace”
- Tracing is made of segments (+ sub segments)
- Annotations can be added to traces to provide extra-information
- Ability to trace:
• Every request
• Sample request (as a % for example or a rate per minute)
- X-Ray Security:
• IAM for authorization
• KMS for encryption at rest
#### AWS X-Ray
How to enable it?
- I) Your code (Java, Python, Go, Node.js, .NET) must import the AWS
X-Ray SDK
• Very little code modification needed
• The application SDK will then capture:
• Calls to AWS services
• HTTP / HTTPS requests
• Database Calls (MySQL, PostgreSQL, DynamoDB)
• Queue calls (SQS)
- 2) Install the X-Ray daemon or enable X-Ray AWS Integration
• X-Ray daemon works as a low level UDP packet interceptor
(Linux / Windows / Mac…)
• AWS Lambda / other AWS services already run the X-Ray
daemon for you
• Each application must have the IAM rights to write data to X-Ray
#### AWS X-Ray Troubleshooting
- If X-Ray is not working on EC2
• Ensure the EC2 IAM Role has the proper permissions
• Ensure the EC2 instance is running the X-Ray Daemon
- To enable on AWS Lambda:
• Ensure it has an IAM execution role with proper policy
(AWSX-RayWriteOnlyAccess)
• Ensure that X-Ray is imported in the code
#### X-Ray Instrumentation in your code
- Instrumentation means the
measure of product’s performance,
diagnose errors, and to write trace
information.
- To instrument your application
code, you use the X-Ray SDK
- Many SDK require only
configuration changes
- You can modify your application
code to customize and annotation
the data that the SDK sends to XRay,
using interceptors, filters,
handlers, middleware…
#### AWS X-Ray Troubleshooting
- If X-Ray is not working on EC2
• Ensure the EC2 IAM Role has the proper permissions
• Ensure the EC2 instance is running the X-Ray Daemon
- To enable on AWS Lambda:
• Ensure it has an IAM execution role with proper policy
(AWSX-RayWriteOnlyAccess)
• Ensure that X-Ray is imported in the code
#### X-Ray Instrumentation in your code
• Instrumentation means the
measure of product’s performance,
diagnose errors, and to write trace
information.
• To instrument your application
code, you use the X-Ray SDK
• Many SDK require only
configuration changes
• You can modify your application
code to customize and annotation
the data that the SDK sends to XRay,
using interceptors, filters,
handlers, middleware…
#### X-Ray Concepts
- Segments: each application / service will send them
- Subsegments: if you need more details in your segment
- Trace: segments collected together to form an end-to-end trace
- Sampling: decrease the amount of requests sent to X-Ray, reduce cost
- Annotations: Key Value pairs used to index traces and use with filters
- Metadata: Key Value pairs, not indexed, not used for searching
- The X-Ray daemon / agent has a config to send traces cross account:
• make sure the IAM permissions are correct – the agent will assume the role
• This allows to have a central account for all your application tracing
#### X-Ray Sampling Rules
- With sampling rules, you control the amount of data that you record
- You can modify sampling rules without changing your code
- By default, the X-Ray SDK records the first request each second, and
five percent of any additional requests.
- One request per second is the reservoir, which ensures that at least one
trace is recorded each second as long the service is serving requests.
- Five percent is the rate at which additional requests beyond the
reservoir size are sampled.
- You can create your own rules with the reservoir and rate
#### X-Ray Write APIs (used by the X-Ray daemon)
- PutTraceSegments: Uploads segment
documents to AWS X-Ray
- PutTelemetryRecords: Used by the AWS
X-Ray daemon to upload telemetry.
• SegmentsReceivedCount,
SegmentsRejectedCounts,
BackendConnectionErrors…
- GetSamplingRules: Retrieve all sampling
rules (to know what/when to send)
• GetSamplingTargets &
GetSamplingStatisticSummaries: advanced
- The X-Ray daemon needs to have an IAM
policy authorizing the correct API calls to
arn:aws:iam::aws:policy/AWSXrayWriteOnlyAccess function correctly
- GetServiceGraph: main graph
- BatchGetTraces: Retrieves a list of
traces specified by ID. Each trace is a
collection of segment documents that
originates from a single request.
- GetTraceSummaries: Retrieves IDs
and annotations for traces available for
a specified time frame using an
optional filter. To get the full traces,
pass the trace IDs to BatchGetTraces.
- GetTraceGraph: Retrieves a service
graph for one or more specific trace
IDs.
####  X-Ray with Elastic Beanstalk
- AWS Elastic Beanstalk platforms include the X-Ray daemon
- You can run the daemon by setting an option in the Elastic Beanstalk console
or with a configuration file (in .ebextensions/xray-daemon.config)
- Make sure to give your instance profile the correct IAM permissions so that
the X-Ray daemon can function correctly
- Then make sure your application code is instrumented with the X-Ray SDK
- Note: The X-Ray daemon is not provided for Multicontainer Docker
### AWS CloudTrail
- Provides governance, compliance and audit for your AWS Account
- CloudTrail is enabled by default!
- Get an history of events / API calls made within your AWS Account by:
• Console
• SDK
• CLI
• AWS Services
- Can put logs from CloudTrail into CloudWatch Logs
- If a resource is deleted in AWS, look into CloudTrail first!

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
