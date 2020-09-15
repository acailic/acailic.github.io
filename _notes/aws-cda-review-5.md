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


