---
title: AWS ECS, ECR & Fargate
layout: post
tags: [aws, cda, ecs, ecr, fargate]
date: 2020-09-16
---

### Docker managment
- To manage containers, we need a container management platform
- Three choices:
•	ECS: Amazon’s own platform
•	Fargate: Amazon’s own Serverless platform
•	EKS: Amazon’s managed Kubernetes (open source)

### ECS
- ECS Clusters are logical grouping of EC2 instances
- EC2 instances run the ECS agent (Docker container)
- The ECS agents registers the instance to the ECS cluster
- The EC2 instances run a special AMI, made specifically for ECS
####  ECS task definition
-	Tasks definitions are metadata in JSON form to tell ECS how to run a Docker Container
-	It contains crucial information around:
•	Image Name
•	Port Binding for Container and Host
•	Memory and CPU required
•	Environment variables
•	Networking information
•	IAM Role
•	Logging configuration (ex CloudWatch)

#### ECS Service
-	ECS Services help define how many tasks should run and how they should be run
-	They ensure that the number of tasks desired is running across our fleet of EC2 instances.
-	They can be linked to ELB / NLB / ALB if needed

### ECR
-	So far we’ve been using Docker images from Docker Hub (public)
-	ECR is a private Docker image repository
-	Access is controlled through IAM (permission errors => policy)
-	AWS CLI v1 login command (may be asked at the exam)
•	$(aws ecr get-login --no-include-email --region eu-west-1)
-	AWS CLI v2 login command (newer, may also be asked at the exam - pipe)
•	aws ecr get-login-password --region eu-west-1 | docker login --username AWS -- password-stdin 1234567890.dkr.ecr.eu-west-1.amazonaws.com
-	Docker Push & Pull:
•	docker push 1234567890.dkr.ecr.eu-west-1.amazonaws.com/demo:latest
•	docker pull 1234567890.dkr.ecr.eu-west-1.amazonaws.com/demo:latest

### Fargate
-	When launching an ECS Cluster, we have to create our EC2 instances
-	If we need to scale, we need to add EC2 instances
-	So we manage infrastructure…
-	With Fargate, it’s all Serverless! We don’t provision EC2 instances
-	We just create task definitions, and AWS will run our containers for us


