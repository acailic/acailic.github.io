---
title: AWS CLI, SDK, IAM roles and policies
layout: post
tags: [aws, cda, cli, sdk, iam]
date: 2020-09-17
---

## AWS CLI, SDK, IAM roles and policies
### Section Introduction
• So far, we’ve interacts with services manually and they exposed standard
information for clients:
• EC2 exposes a standard Linux machine we can use any way we want
• RDS exposes a standard database we can connect to using a URL
• ElastiCache exposes a cache URL we can connect to using a URL
• ASG / ELB are automated and we don’t have to program against them
• Route53 was setup manual
• Developing against AWS has two components:
• How to perform interactions with AWS without using the Online Console?
• How to interact with AWS Proprietary services? (S3, DynamoDB, etc…)
• Developing and performing AWS tasks against AWS can be done in
several ways
• Using the AWS CLI on our local computer
• Using the AWS CLI on our EC2 machines
• Using the AWS SDK on our local computer
• Using the AWS SDK on our EC2 machines
• Using the AWS Instance Metadata Service for EC2
• In this section, we’ll learn:
• How to do all of those
• In the right & most secure way, adhering to best practices
### CLI Installation Troubleshooting
• If after installing the AWS CLI, you use it and you get the error
aws: command not found
• Then, on Linux, Mac and Windows
the aws executable is not in the PATH environment variable
• PATH allows your system to know where to find the “aws” executable
### AWS CLI Configuration
• Let’s learn how to properly configure the CLI 
• Do not share your AWS Access Key and Secret key with anyone 
AWS CLI ON EC2… THE BADWAY
• We could run `aws configure` on EC2 just like we did (and it’ll work)
• But… it’s SUPER INSECURE
• NEVER EVER EVER PUT YOUR PERSONAL CREDENTIALS ON AN EC2
• Your PERSONAL credentials are PERSONAL and only belong on your
PERSONAL computer
• If the EC2 is compromised, so is your personal account
• If the EC2 is shared, other people may perform AWS actions while
impersonating you
• For EC2, there’s a better way… it’s called AWS IAM Roles
## AWS CLI ON EC2… THE RIGHT WAY
• IAM Roles can be attached to EC2 instances
• IAM Roles can come with a policy authorizing exactly what the EC2 instance
should be able to do
• EC2 Instances can then use these profiles automatically without any additional
configurations
• This is the best practice on AWS and you should 100% do this.
### AWS CLI Dry Runs
• Sometimes, we’d just like to make sure we have the permissions…
• But not actually run the commands!
• Some AWS CLI commands (such as EC2) can become expensive if they
succeed, say if we wanted to try to create an EC2 Instance
• Some AWS CLI commands (not all) contain a --dry-run option to
simulate API calls
### AWS CLI STS Decode Errors
• When you run API calls and they fail, you can get a long error message
• This error message can be decoded using the STS command line:
• sts decode-authorization-message
#### AWS EC2 Instance Metadata
• AWS EC2 Instance Metadata is powerful but one of the least known features
to developers
• It allows AWS EC2 instances to ”learn about themselves” without using an
IAM Role for that purpose.
• The URL is http://169.254.169.254/latest/meta-data
• You can retrieve the IAM Role name from the metadata, but you CANNOT
retrieve the IAM Policy.
• Metadata = Info about the EC2 instance
• Userdata = launch script of the EC2 instance
#### MFA with CLI
• To use MFA with the CLI, you must create a temporary session
• To do so, you must run the STS GetSessionToken API call
• aws sts get-session-token --serial-number arn-of-the-mfa-device --tokencode
code-from-token --duration-seconds 3600
#### 
