---
title: AWS Elastic Beanstalk
layout: post
tags: [aws, cda, elastic, beanstalk]
date: 2020-09-16
---

## AWS Elastic Beanstalk
### intro
-	Managing infrastructure
-	Deploying Code. Configuring all the databases, load balancers. Scaling concerns.
-	Most web apps have the same architecture (ALB + ASG). Possibly, consistently across different applications and environments
- https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/AWSHowTo.html

## AWS Elastic Beanstalk	Overview
-	Elastic Beanstalk is a developer centric view of deploying an application on AWS
- It uses all the component’s we’ve seen before: EC2, ASG, ELB, RDS, etc…
- it’s all in one view. We still have full control over the configuration
-	Beanstalk is free but you pay for the underlying instances
### Elastic Beanstalk
- Managed service. Supports many platforms, docker containers.
- Instance configuration / OS is handled by Beanstalk. Deployment strategy is configurable but performed by Elastic Beanstalk
- Just the application code is the responsibility of the developer
-	Three architecture models:
•	Single Instance deployment: good for dev
•	LB + ASG: great for production or pre-production web applications
•	ASG only: great for non-web apps in production (workers, etc..)
-	Elastic Beanstalk has three components
•	Application
•	Application version: each deployment gets assigned a version
•	Environment name (dev, test, prod…): free naming
-	You deploy application versions to environments and can promote application versions to the next environment
-	Rollback feature to previous application version
- Create appl + Create environtmets --> upload version --> release
### Beanstalk Deployment modes
- All at once (deploy all in one go) – fastest, but instances aren’t available to serve traffic for a bit (downtime). Great for quick iteration in devolopment. No additional costs.
- Rolling: update a few instances at a time (bucket), and then move onto the next bucket once the first bucket is healthy. App is running below capacity, can set bucket size. App is running both versions simultaneously. No additional costs, log deployment.
- Rolling with additional batches: like rolling, but spins up new instances to move the batch (so that the old application is still available). App is running at capacity, can set bucket size, both versions simultaneously. Small additional costs. Additional batch is removed at the end of deployment, good for prod.
- Immutable: spins up new instances in a new ASG, deploys version to these instances, and then swaps all the instances when everything is healthy. Zero downtime, High cost, double capacity, longest deployment. New code is deployed to new instances on temporary ASG. Quick rollback in case of failure, great for prod.
- Blue / Green - Not a “direct feature” of Elastic Beanstalk. Zero downtime and release facility.	Create a new “stage” environment and deploy v2 there. The new environment (green) can be validated independently and roll back if issues. Route 53 can be setup using weighted policies to redirect a little bit of traffic to the stage environment. Using Beanstalk, “swap URLs” when done with the environment test

### Elastic Beanstalk CLI
- We can install an additional CLI called the “EB cli” which makes working with Beanstalk from the CLI easier. 
-	Basic commands are:
•	eb create
•	eb status
•	eb health
•	eb events
•	eb logs
•	eb open
•	eb deploy
•	eb config
•	eb terminate
#### Deployment process
- Describe dependencies(requirements.txt for Python, package.json for Node.js)
- Package code as zip, and describe dependencies
- Python: requirements.txt
- Node.js: package.json
- Console: upload zip file (creates new app version), and then deploy
- CLI: create new app version using CLI (uploads zip), and then deploy
- Elastic Beanstalk will deploy the zip on each EC2 instance, resolve dependencies and start the application

### Beanstalk Lifecycle Policy and Extension
- Elastic Beanstalk can store at most 1000 application versions. If you don’t remove old versions, you won’t be able to deploy anymore
- To phase out old application versions, use a lifecycle policy:Based on time (old versions are removed),Based on space (when you have too many versions)
-	Versions that are currently used won’t be deleted. Option not to delete the source bundle in S3 to prevent data loss
- A zip file containing our code must be deployed to Elastic Beanstalk
- All the parameters set in the UI can be configured with code using files
- Requirements:
•	in the .ebextensions/ directory in the root of source code
•	YAML / JSON format
•	.config extensions (example: logging.config)
•	 Able to modify some default settings using: option_settings
•	Ability to add resources such as RDS, ElastiCache, DynamoDB, etc…
- Resources managed by .ebextensions get deleted if the environment goes away

### Elastic Beanstalk
#### Under the hood
- Under the hood, Elastic Beanstalk relies on CloudFormation. CloudFormation is used to provision other AWS services 
- Use case: you can define CloudFormation resources in your .ebextensions to provision ElastiCache, an S3 bucket

#### Cloning
- Clone an environment with the exact same configuration
-	Useful for deploying a “test” version of your application
 - All resources and configuration are preserved:
•	Load Balancer type and configuration
•	RDS database type (but the data is not preserved)
•	Environment variables
-	After cloning an environment, you can change settings

#### Migration
#### RDS
#### Docker
##### Single Docker
##### Multiple Docker
#### Web Server vs Worker Environment
- If your application performs tasks that are long to complete, offload these tasks to a dedicated
worker environment
-	Decoupling your application into two tiers is common. Requests into ALB+ASG further put into SQS Queuqe + ASG
-	Example: processing a video, generating a zip file, etc. You can define periodic tasks in a file cron.yaml

