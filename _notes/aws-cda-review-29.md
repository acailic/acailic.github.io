---
title: AWS Other sevices SES, ACM
layout: post
tags: [aws, cda, ses, acm]
date: 2020-09-14
---

## Other AWS Services
### AWS Certificate Manager (ACM)
- To host public SSL certificates in AWS, you can:
• Buy your own and upload them using the CLI
• Have ACM provision and renew public SSL
certificates for you (free of cost)
- ACM loads SSL certificates on the following
integrations:
• Load Balancers (including the ones created by EB)
• CloudFront distributions
• APIs on API Gateways
- SSL certificates is overall a pain to manually
manage, to ACM is great to leverage in your
AWS infrastructure!
- Less CPU cost in EC2
Thanks to SSL termination for the ELB
### AWS SWF – Simple Workflow Service
- Coordinate work amongst applications
- Code runs on EC2 (not serverless)
- 1 year max runtime
- Concept of “activity step” and “decision step”
- Has built-in “human intervention” step
- Example: order fulfilment from web to warehouse to delivery
- Step Functions is recommended to be used for new applications, except:
• If you need external signals to intervene in the processes
• If you need child processes that return values to parent processes
### AWS SES – Simple Email Service
- Send emails to people using:
• SMTP interface
• Or AWS SDK
- Ability to receive email. Integrates with:
• S3
• SNS
• Lambda
- Integrated with IAM for allowing to send emails
###  AWS Databases Summary
- RDS: Relational databases, OLTP
• PostgreSQL, MySQL, Oracle…
• Aurora + Aurora Serverless
• Provisioned database
- DynamoDB: NoSQL DB
• Managed, Key Value, Document
• Serverless
- ElastiCache: In memory DB
• Redis / Memcached
• Cache capability, provisoned
- Redshift: OLAP – Analytic Processing
• Data Warehousing / Data Lake
• Analytics queries
- Neptune: Graph Database
- DMS: Database Migration Service
- DocumentDB: managed MongoDB
for AWS

### Questions
- You need to load SSL certificates onto your Load Balancers and also have EC2 instances dynamically retrieve them when needed for service to service two way TLS communication. What service should you use to centrally manage these SSL certificates?-ACM
