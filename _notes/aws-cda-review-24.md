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

### AWS SDK Overview
• What if you want to perform actions on AWS directly from your applications
code ? (without using the CLI).
• You can use an SDK (software development kit) !
• Official SDKs are…
• Java
• .NET
• Node.js
• PHP
• Python (named boto3 / botocore)
• Go
• Ruby
• C++
#### • We have to use the AWS SDK when coding against AWS Services such
as DynamoDB
• Fun fact… the AWS CLI uses the Python SDK (boto3)
• The exam expects you to know when you should use an SDK
• We’ll practice the AWS SDK when we get to the Lambda functions
• Good to know: if you don’t specify or configure a default region, then
us-east-1 will be chosen by default
#### AWS Limits (Quotas)
• API Rate Limits
• DescribeInstances API for EC2 has a limit of 100 calls per seconds
• GetObject on S3 has a limit of 5500 GET per second per prefix
• For Intermittent Errors: implement Exponential Backoff
• For Consistent Errors: request an API throttling limit increase
• Service Quotas (Service Limits)
• Running On-Demand Standard Instances: 1152 vCPU
• You can request a service limit increase by opening a ticket
• You can request a service quota increase by using the Service Quotas API
#### Exponential Backoff (any AWS service)
• If you get ThrottlingException intermittently, use exponential backoff
• Retry mechanism included in SDK API calls
• Must implement yourself if using the API as is or in specific cases
#### AWS CLI Credentials Provider Chain
• The CLI will look for credentials in this order
1. Command line options – --region, --output, and --profile
2. Environment variables – AWS_ACCESS_KEY_ID,AWS_SECRET_ACCESS_KEY,
and AWS_SESSION_TOKEN
3. CLI credentials file –aws configure
~/.aws/credentials on Linux / Mac & C:\Users\user\.aws\credentials on Windows
4. CLI configuration file – aws configure
~/.aws/config on Linux / macOS & C:\Users\USERNAME\.aws\config on Windows
5. Container credentials – for ECS tasks
6. Instance profile credentials – for EC2 Instance Profiles
#### AWS SDK Default Credentials Provider Chain
• The Java SDK (example) will look for credentials in this order
1. Environment variables –
AWS_ACCESS_KEY_ID and AWS_SECRET_ACCESS_KEY
2. Java system properties – aws.accessKeyId and aws.secretKey
3. The default credential profiles file – ex at: ~/.aws/credentials, shared by
many SDK
4. Amazon ECS container credentials – for ECS containers
5. Instance profile credentials– used on EC2 instances
#### AWS Credentials Scenario
• An application deployed on an EC2 instance is using environment variables
with credentials from an IAM user to call the Amazon S3 API.
• The IAM user has S3FullAccess permissions.
• The application only uses one S3 bucket, so according to best practices:
• An IAM Role & EC2 Instance Profile was created for the EC2 instance
• The Role was assigned the minimum permissions to access that one S3 bucket
• The IAM Instance Profile was assigned to the EC2 instance, but it still had
access to all S3 buckets. Why?
the credentials chain is still giving priorities to the environment variables
#### AWS Credentials Best Practices
• Overall, NEVER EVER STORE AWS CREDENTIALS IN YOUR CODE
• Best practice is for credentials to be inherited from the credentials chain
• If using working within AWS, use IAM Roles
• => EC2 Instances Roles for EC2 Instances
• => ECS Roles for ECS tasks
• => Lambda Roles for Lambda functions
• If working outside of AWS, use environment variables / named profiles
#### Signing AWS API requests
• When you call the AWS HTTP API, you sign the request so that AWS
can identify you, using your AWS credentials (access key & secret key)
• Note: some requests to Amazon S3 don’t need to be signed
• If you use the SDK or CLI, the HTTP requests are signed for you
• You should sign an AWS HTTP request using Signature v4 (SigV4)
#### S3 MFA-Delete
• MFA (multi factor authentication) forces user to generate a code on a device (usually a
mobile phone or hardware) before doing important operations on S3
• To use MFA-Delete, enable Versioning on the S3 bucket
• You will need MFA to
• permanently delete an object version
• suspend versioning on the bucket
• You won’t need MFA for
• enabling versioning
• listing deleted versions
• Only the bucket owner (root account) can enable/disable MFA-Delete
• MFA-Delete currently can only be enabled using the CLI
#### S3 Default Encryption vs Bucket Policies
• The old way to enable default encryption was to use a bucket policy
and refuse any HTTP command without the proper headers
• The new way is to use the “default encryption” option in S3
• Note: Bucket Policies are evaluated before “default encryption”
#### S3 Access Logs
• For audit purpose, you may want to log all access to S3
buckets
• Any request made to S3, from any account, authorized or
denied, will be logged into another S3 bucket
• That data can be analyzed using data analysis tools…
• Or Amazon Athena as we’ll see later in this section!
• The log format is at:
https://docs.aws.amazon.com/AmazonS3/latest/dev/LogFo
rmat.html
#### S3 Access Logs: Warning
• Do not set your logging bucket to be the monitored bucket
• It will create a logging loop, and your bucket will grow in size exponentially
#### S3 Replication (CRR & SRR)
• Must enable versioning in source and destination
• Cross Region Replication (CRR)
• Same Region Replication (SRR)
• Buckets can be in different accounts
• Copying is asynchronous
• Must give proper IAM permissions to S3
• CRR - Use cases: compliance, lower latency access,
replication across accounts
• SRR – Use cases: log aggregation, live replication
between production and test accounts
#### S3 Replication – Notes
• After activating, only new objects are replicated (not retroactive)
• For DELETE operations:
• If you delete without a version ID, it adds a delete marker, not replicated
• If you delete with a version ID, it deletes in the source, not replicated
• There is no “chaining” of replication
• If bucket 1 has replication into bucket 2, which has replication into bucket 3
• Then objects created in bucket 1 are not replicated to bucket 3
#### S3 Pre-Signed URLs
• Can generate pre-signed URLs using SDK or CLI
• For downloads (easy, can use the CLI)
• For uploads (harder, must use the SDK)
• Valid for a default of 3600 seconds, can change timeout with --expires-in
[TIME_BY_SECONDS] argument
• Users given a pre-signed URL inherit the permissions of the person who
generated the URL for GET / PUT
• Examples :
• Allow only logged-in users to download a premium video on your S3 bucket
• Allow an ever changing list of users to download files by generating URLs dynamically
• Allow temporarily a user to upload a file to a precise location in our bucket
#### 
