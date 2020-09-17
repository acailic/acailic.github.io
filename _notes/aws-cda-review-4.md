---
title: AWS ECS, ECR & Fargate
layout: post
tags: [aws, cda, ecs, ecr, fargate]
date: 2020-09-16
---

### Docker managment
- To manage containers, we need a container management platform
- Three choices:
•	ECS: Amazon’s own platform, “Classic” provision EC2 instances to run containers onto
•	Fargate: Amazon’s own Serverless platform, ECS Serverless, no more EC2 to provision
•	EKS: Amazon’s managed Kubernetes (open source)
## ECS
- ECS Clusters are logical grouping of EC2 instances
- EC2 instances run the ECS agent (Docker container)
- The ECS agents registers the instance to the ECS cluster
- The EC2 instances run a special AMI, made specifically for ECS
-	EC2 instances must be created
- We must configure the file /etc/ecs/ecs.config with the cluster name
-	The EC2 instance must run an ECS agent
-	EC2 instances can run multiple containers on the same type:
•	You must not specify a host port (only container port)
•	You should use an Application Load Balancer with the dynamic port mapping
•	The EC2 instance security group must allow traffic from the ALB on all ports
-	ECS tasks can have IAM Roles to execute actions against AWS
-	Security groups operate at the instance level, not task level
-	ECS does integrate with CloudWatch Logs:
•	You need to setup logging at the task definition level
•	Each container will have a different log stream
•	The EC2 Instance Profile needs to have the correct IAM permissions

#### ECS task definition
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
#### ECS Task Placement
-	When a task of type EC2 is launched, ECS must determine where to place it, with the constraints of CPU, memory, and available port. Similarly, when a service scales in, ECS needs to determine which task to terminate.
-	To assist with this, you can define a task placement strategy and task placement constraints
-	Note: this is only for ECS with EC2, not for Fargate
#### ECS Task Placement Process
-Task placement strategies are a best effort
-	When Amazon ECS places tasks, it uses the following process to select container instances:
1.	Identify the instances that satisfy the CPU, memory, and port requirements in the task definition.
2.	Identify the instances that satisfy the task placement constraints.
3.	Identify the instances that satisfy the task placement strategies.
4.	Select the instances for task placement.
-	Binpack: place tasks based on the least available amount of CPU or memory. This minimizes the number of instances in use (cost savings)
- Random
- Spread: place the task evenly based on the specified value. Example: instanceId, attribute:ecs.availability-zone
- Mix them together.
##### ECS Task Placement Constraints
- distinctInstance: place each task on a different container instance
- memberOf: places task on instances that satisfy an expression
•	Uses the Cluster Query Language (advanced)
#### ECS – Service Auto Scaling
- CPU and RAM is tracked in CloudWatch at the ECS service level
- Target Tracking: target a specific average CloudWatch metric
- Step Scaling: scale based on CloudWatch alarms
- Scheduled Scaling: based on predictable changes
- ECS Service Scaling (task level) ≠ EC2 Auto Scaling (instance level)
- Fargate Auto Scaling is much easier to setup (because serverless)
#### ECS IAM Role
- EC2 Instance Profile:
•	Used by the ECS agent
•	Makes API calls to ECS service
•	Send container logs to CloudWatch Logs
•	Pull Docker image from ECR
- ECS Task Role:
•	Allow each task to have a specific role
•	Use different roles for the different ECS Services you run
•	Task Role is defined in the task definition
#### ECS CLuster 
•	A Capacity Provider is used in association with a cluster to determine the infrastructure that a task runs on
•	For ECS and Fargate users, the FARGATE and FARGATE_SPOT capacity providers are added automatically
•	For Amazon ECS on EC2, you need to associate the capacity provider with an auto-scaling group
•	When you run a task or a service, you define a capacity provider strategy, to prioritize in which provider to run.
•	This allows the capacity provider to automatically provision infrastructure for you
## ECR
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
-	Fargate is Serverless (no EC2 to manage)
-	AWS provisions containers for us and assigns them ENI
-	Fargate containers are provisioned by the container spec (CPU / RAM)
-	Fargate tasks can have IAM Roles to execute actions against AWS

### Questions
- Which ECS config must you enable in /etc/ecs/ecs.config to allow your ECS tasks to endorse IAM roles ? - ECS_ENABLE_TASK_IAM_ROLE
- Any permissions issues against ECR is most likely due to IAM policies.
- run multiple instances of the same application on the same EC2 instance and expose it with a load balancer. The application is available as a Docker container. - Use ALB+ECS, uses dynamic port feature
-  running a web application on ECS, the Docker image is stored on ECR, and trying to launch two containers of the same type on EC2. The first container starts, but the second one doesn't. You have checked and there's enough CPU and RAM available on the EC2 instance - Host port is defined in thas definition, To enable random host port, set host port = 0 (or empty), which allows multiple containers of the same type to launch on the same instance
-  EC2 instance and it's not registered with the ECS cluster. What's NOT a reason for this issue? - security groups do not matter when an instance registers with the ECS service
- pull an image from ECR? (CLI v1) : 1. $ (aws ecr get-login --no-include-email) ; 2. docker pull $ECR_IMAGE_URL
- to run 4 ECS services on your ECS cluster, which need access to various services. What is the best practice - create 4 ECS task roles and attache them to relevant ECS task definition
- task cluster placement is the MOST cost-efficient - binpack






