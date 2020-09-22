---
title: AWS Integration & Messaging
layout: post
tags: [aws, cda, integration, messaging]
date: 2020-09-17
---

### Intro
1) Synchronous communicaions
(applicaion to applicaion)
2) Asynchronous / Event based
(applicaion to queue to applicaion)
-Synchronous between applications can be problematic if there are
sudden spikes of traffic
• What if you need to suddenly encode 1000 videos but usually it’s 10?
• In that case, it’s better to decouple your applications,
• using SQS: queue model
• using SNS: pub/sub model
• using Kinesis: real-time streaming model

### Amazon SQS – Standard Queue
• Oldest offering (over 10 years old)
• Fully managed service, used to decouple applications
• Attributes:
• Unlimited throughput, unlimited number of messages in queue
• Default retention of messages: 4 days, maximum of 14 days
• Low latency (<10 ms on publish and receive)
• Limitation of 256KB per message sent
• Can have duplicate messages (at least once delivery, occasionally)
• Can have out of order messages (best effort ordering)
#### SQS – Producing Messages
• Produced to SQS using the SDK (SendMessage API)
• The message is persisted in SQS until a consumer deletes it
• Message retention: default 4 days, up to 14 days
• Example: send an order to be processed
• Order id
• Customer id
• Any attributes you want
• SQS standard: unlimited throughput
#### SQS – Consuming Messages
• Consumers (running on EC2 instances, servers, or AWS Lambda)…
• Poll SQS for messages (receive up to 10 messages at a time)
• Process the messages (example: insert the message into an RDS database)
• Delete the messages using the DeleteMessage API
#### SQS – Multiple EC2 Instances Consumers
Consumers receive and process
messages in parallel
• At least once delivery
• Best-effort message ordering
• Consumers delete messages
after processing them
• We can scale consumers
horizontally to improve
throughput of processing
#### SQS with Auto Scaling Group (ASG)
#### SQS to decouple between application tiers
#### Amazon SQS - Security
• Encryption:
• In-flight encryption using HTTPS API
• At-rest encryption using KMS keys
• Client-side encryption if the client wants to perform encryption/decryption itself
• Access Controls: IAM policies to regulate access to the SQS API
• SQS Access Policies (similar to S3 bucket policies)
• Useful for cross-account access to SQS queues
• Useful for allowing other services (SNS, S3…) to write to an SQS queue
#### SQS – Message Visibility Timeout
• After a message is polled by a consumer, it becomes invisible to other consumers
• By default, the “message visibility timeout” is 30 seconds
• That means the message has 30 seconds to be processed
• After the message visibility timeout is over, the message is “visible” in SQS
If a message is not processed within the visibility timeout, it will be processed twice
• A consumer could call the ChangeMessageVisibility API to get more time
• If visibility timeout is high (hours), and consumer crashes, re-processing will take time
• If visibility timeout is too low (seconds), we may get duplicates
#### Amazon SQS – Dead Letter Queue
• If a consumer fails to process a message within the
Visibility Timeout…
the message goes back to the queue!
• We can set a threshold of how many times a message
can go back to the queue
• After the MaximumReceives threshold is exceeded, the
message goes into a dead letter queue (DLQ)
• Useful for debugging!
• Make sure to process the messages in the DLQ before
they expire:
• Good to set a retention of 14 days in the DLQ
#### Amazon SQS – Delay Queue
• Delay a message (consumers don’t see it immediately) up to 15 minutes
• Default is 0 seconds (message is available right away)
• Can set a default at queue level
• Can override the default on send using the DelaySeconds parameter
#### Amazon SQS - Long Polling
• When a consumer requests messages from the
queue, it can optionally “wait” for messages to
arrive if there are none in the queue
• This is called Long Polling
• LongPolling decreases the number of API calls
made to SQS while increasing the efficiency and
latency of your application.
• The wait time can be between 1 sec to 20 sec
(20 sec preferable)
• Long Polling is preferable to Short Polling
• Long polling can be enabled at the queue level or at the API level using WaitTimeSeconds
#### SQS Extended Client
• Message size limit is 256KB, how to send large messages, e.g. 1GB?
• Using the SQS Extended Client (Java Library)
#### SQS – Must know API
• CreateQueue (MessageRetentionPeriod), DeleteQueue
• PurgeQueue: delete all the messages in queue
• SendMessage (DelaySeconds), ReceiveMessage, DeleteMessage
• ReceiveMessageWaitTimeSeconds: Long Polling
• ChangeMessageVisibility: change the message timeout
• Batch APIs for SendMessage, DeleteMessage, ChangeMessageVisibility
helps decrease your costs
#### Amazon SQS – FIFO Queue
Limited throughput: 300 msg/s without batching, 3000 msg/s with
• Exactly-once send capability (by removing duplicates)
• Messages are processed in order by the consumer
#### SQS FIFO – Deduplication
• FIFO = First In First Out (ordering of messages in the queue)
• De-duplication interval is 5 minutes
- Two de-duplication methods:
• Content-based deduplication: will do a SHA-256 hash of the message body
• Explicitly provide a Message Deduplication ID
#### SQS FIFO – Message Grouping
• If you specify the same value of MessageGroupID in an SQS FIFO queue,
you can only have one consumer, and all the messages are in order
• To get ordering at the level of a subset of messages, specify different values
for MessageGroupID
• Messages that share a common Message Group ID will be in order within the group
• Each Group ID can have a different consumer (parallel processing!)
• Ordering across groups is not guaranteed
