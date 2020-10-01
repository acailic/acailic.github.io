---
title: AWS CDA Practise test 2
layout: post
tags: [aws, cda, test]
date: 2020-08-17
---

## Deployment
- AWS::Serverless::UserPool
The AWS Serverless Application Model (SAM) is an open-source framework for building serverless applications. It provides shorthand syntax to express functions, APIs, databases, and event source mappings. With just a few lines per resource, you can define the application you want and model it using YAML.
SAM supports the following resource types:
AWS::Serverless::Api
AWS::Serverless::Application
AWS::Serverless::Function
AWS::Serverless::HttpApi
AWS::Serverless::LayerVersion
AWS::Serverless::SimpleTable
AWS::Serverless::StateMachine
UserPool applies to the Cognito service which is used for authentication for mobile app and web. There is no resource named UserPool in the Serverless Application Model.
- 'Dependencies' section of the template - As you can see, there is no section called 'Dependencies' in the template. Although dependencies can be mentioned, there is no section itself for dependencies.
'Conditions' section of the template - This optional section includes conditions that control whether certain resources are created or whether certain resource properties are assigned a value during stack creation or update. For example, you could conditionally create a resource that depends on whether the stack is for a production or test environment.
'Resources' section of the template - This is the only required section and specifies the stack resources and their properties, such as an Amazon Elastic Compute Cloud instance or an Amazon Simple Storage Service bucket. You can refer to resources in the Resources and Outputs sections of the template.
'Parameters' section of the template - This optional section is helpful in passing Values to your template at runtime (when you create or update a stack). You can refer to parameters from the Resources and Outputs sections of the template.
- Rolling- With Elastic Beanstalk, you can quickly deploy and manage applications in the AWS Cloud without having to learn about the infrastructure that runs those applications. Elastic Beanstalk reduces management complexity without restricting choice or control. You simply upload your application, and Elastic Beanstalk automatically handles the details of capacity provisioning, load balancing, scaling, and application health monitoring.
- IAM username and password - IAM username and password credentials cannot be used to access CodeCommit.
Git credentials - These are IAM -generated user name and password pair you can use to communicate with CodeCommit repositories over HTTPS.
SSH Keys - Are locally generated public-private key pair that you can associate with your IAM user to communicate with CodeCommit repositories over SSH.
AWS access keys - You can use these keys with the credential helper included with the AWS CLI to communicate with CodeCommit repositories over HTTPS.
- Create a cross-stack reference and use the Export output field to flag the value of VPC from the network stack. Then use Fn::ImportValue intrinsic function to import the value of VPC into the web application stack. AWS CloudFormation gives developers and businesses an easy way to create a collection of related AWS and third-party resources and provision them in an orderly and predictable fashion.
- "Exported Output Values in CloudFormation must have unique names within a single Region".Using CloudFormation, you can create a template that describes all the AWS resources that you want (like Amazon EC2 instances or Amazon RDS DB instances), and AWS CloudFormation takes care of provisioning and configuring those resources for you.
A CloudFormation template has an optional Outputs section which declares output values that you can import into other stacks (to create cross-stack references), return in response (to describe stack calls), or view on the AWS CloudFormation console. For example, you can output the S3 bucket name for a stack to make the bucket easier to find.
You can use the Export Output Values to export the name of the resource output for a cross-stack reference. For each AWS account, export names must be unique within a region. In this case, we would have a conflict within us-east-2.

## Monitoring
## Security
## Refactoring
## Development
## ECS
- ECS:Amazon Elastic Container Service (Amazon ECS) is a highly scalable, fast, container management service that makes it easy to run, stop, and manage Docker containers on a cluster. You can host your cluster on a serverless infrastructure that is managed by Amazon ECS by launching your services or tasks using the Fargate launch type. For more control over your infrastructure, you can host your tasks on a cluster of Amazon Elastic Compute Cloud (Amazon EC2) instances that you manage by using the EC2 launch type.
Amazon ECS can be used to create a consistent deployment and build experience, manage, and scale batch and Extract-Transform-Load (ETL) workloads, and build sophisticated application architectures on a microservices model.
Amazon Elastic Container Registry (ECR) is a fully-managed Docker container registry that makes it easy for developers to store, manage, and deploy Docker container images. Amazon ECR is integrated with Amazon Elastic Container Service (ECS), simplifying your development to production workflow.
