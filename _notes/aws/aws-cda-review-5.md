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
- it uploads zip to s3

### Beanstalk Lifecycle Policy and Extension
- Elastic Beanstalk can store at most 1000 application versions. If you don’t remove old versions, you won’t be able to deploy anymore
- To phase out old application versions, use a lifecycle policy:Based on time (old versions are removed),Based on space (when you have too many versions)
-	Versions that are currently used won’t be deleted. Option not to delete the source bundle in S3 to prevent data loss
- A zip file containing our code must be deployed to Elastic Beanstalk
- All the parameters set in the UI can be configured with code using files
- Requirements, so must be :
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
- After creating an Elastic Beanstalk environment, you cannot change the Elastic Load Balancer type (only the configuration)
-	To migrate:
1.	create a new environment with the same configuration except LB (can’t clone)
2.	deploy your application onto the new environment
3.	perform a CNAME swap or Route 53 update

#### RDS
- RDS can be provisioned with Beanstalk, which is great for dev / test
- This is not great for prod as the database lifecycle is tied to the Beanstalk environment lifecycle
- The best for prod is to separately create an RDS database and provide our EB application with the connection string
- To migrate RDS:
1.create a snapshot of RDS DB (as a safeguard)
2.	Go to the RDS console and protect the RDS database from deletion
3.	Create a new Elastic Beanstalk environment, without RDS, point your application to existing RDS
4.	perform a CNAME swap (blue/green) or Route 53 update, confirm working
5.	Terminate the old environment (RDS won’t be deleted)
6.	Delete CloudFormation stack (in DELETE_FAILED state)

#### Docker
##### Single Docker
- Run your application as a single docker container
-	Either provide:
•	Dockerfile: Elastic Beanstalk will build and run the Docker container
•	Dockerrun.aws.json (v1): Describe where *already built* Docker image is
•	Image
•	Ports
•	Volumes
•	Logging
•	Etc...
-	Beanstalk in Single Docker Container does not use ECS

##### Multiple Docker
- Multi Docker helps run multiple containers per EC2 instance in EB
- This will create for you:
•	ECS Cluster
•	EC2 instances, configured to use the ECS Cluster
•	Load Balancer (in high availability mode)
•	Task definitions and execution
- Requires a config Dockerrun.aws.json (v2) at the root of source code
- Dockerrun.aws.json is used to generate the ECS task definition
- Your Docker images must be pre-built and stored in ECR for example

##### Custom Platform
- uses  Packer
- Custom Platforms are very advanced, they allow to define from scratch:
•	The Operating System (OS)
•	Additional Software
•	Scripts that Beanstalk runs on these platforms
-	Use case: app language is incompatible with Beanstalk & doesn’t use Docker
-	To create your own platform:
•	Define an AMI using Platform.yaml file
•	Build that platform using the Packer software (open source tool to create AMIs)
- Custom Platform vs Custom Image (AMI): custom platform is entire new and custom image is a tweak to exisiting platform

##### HTTPS
- Beanstalk with HTTPS
•	Idea: Load the SSL certificate onto the Load Balancer
•	Can be done from the Console (EB console, load balancer configuration)
•	Can be done from the code: .ebextensions/securelistener-alb.config
•	SSL Certificate can be provisioned using ACM (AWS Certificate Manager) or CLI
•	Must configure a security group rule to allow incoming port 443 (HTTPS port)
- Beanstalk redirect HTTP to HTTPS
•	Configure your instances to redirect HTTP to HTTPS: https://github.com/awsdocs/elastic-beanstalk-samples/tree/master/configuration-files/aws-provided/security-configuration/https-redirect
•	OR configure the Application Load Balancer (ALB only) with a rule
•	Make sure health checks are not redirected (so they keep giving 200 OK)

#### Web Server vs Worker Environment
- If your application performs tasks that are long to complete, offload these tasks to a dedicated
worker environment
-	Decoupling your application into two tiers is common. Requests into ALB+ASG further put into SQS Queuqe + ASG
-	Example: processing a video, generating a zip file, etc. You can define periodic tasks in a file cron.yaml

### Questions
- I would like to customize the runtime of Elastic Beanstalk and include some of my company wide security software. I should --> provide custom platform.
- Environments in Elastic Beanstalk can be named freely.
- Deployment to roll back quickly, this deployment mode terminates the temporary ASG that has the new version, while the current one is untouched and already running at capacity.
- to create an ElastiCache with my Elastic Beanstalk environment i should create an elasicchache.config file in the .ebexstensions folder which is root of the code zup file and provide appropriate configuration
- Elastic Beanstalk have been painfully slow, and after looking at the logs, I realize this is due to the fact that my dependencies are resolved on each EC2 machine at deployment time. How can I speed up my deployment with the minimal impact? - Resolve dependencies beforehand and package them in the zip file uploaded to ElasticBeanStalk
- your Elastic Beanstalk environment to expose an HTTPS endpoint instead of an HTTP endpoint in order to get in-flight encryption between your clients and your web servers. What must be done to setup HTTPS on Beanstalk? - Create an .ebextension/elb.config file to configure the load balancer
- how can you remove older versions that are not used by Elastic Beanstalk so that new versions can be created for your applications - Use lifecycle policy
- https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/using-features-managing-env-tiers.html
- To perform a set of repetitive and scheduled tasks asynchronously. Which Elastic Beanstalk environment should you setup? - Setup a worker environment and a cron.yaml file.
- You have created a test environment in Elastic Beanstalk and as part of that environment, you have created an RDS database. How can you make sure the database can be explored after the environment is destroyed ? - Make a snapshot of the db before its deleted 
