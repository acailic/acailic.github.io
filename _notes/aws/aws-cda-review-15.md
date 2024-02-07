---
title: AWS Advanced identity
layout: post
tags: [aws, cda, iam]
date: 2020-09-17
---
### AWS STS – Security Token Service
- Allows to grant limited and temporary access to AWS resources (up to 1 hour).
- AssumeRole: Assume roles within your account or cross account
- AssumeRoleWithSAML: return credentials for users logged with SAML
- AssumeRoleWithWebIdentity
• return creds for users logged with an IdP (Facebook Login, Google Login, OIDC compatible…)
• AWS recommends against using this, and using Cognito Identity Pools instead
- GetSessionToken: for MFA, from a user or AWS account root user
- GetFederationToken: obtain temporary creds for a federated user
- GetCallerIdentity: return details about the IAM user or role used in the API call
- DecodeAuthorizationMessage: decode error message when an AWS API is denied
### Using STS to Assume a Role
- Define an IAM Role within your
account or cross-account
- Define which principals can access
this IAM Role
- Use AWS STS (Security Token
Service) to retrieve credentials and
impersonate the IAM Role you
have access to (AssumeRole API)
- Temporary credentials can be valid
between 15 minutes to 1 hour
### Cross account access with STS
- https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_common-scenarios.html
- it gets roles credential per group like developers and testers
### STS with MFA
- Use GetSessionToken from STS
- Appropriate IAM policy using
IAM Conditions
- aws:MultiFactorAuthPresent:true
- Reminder, GetSessionToken
returns:
• Access ID
• Secret Key
• Session Token
• Expiration date
### Advanced IAM- Auth Model, evaluation of policies simplified
1. if there an explicit DENY, end decision and DENY
2. if there is ALLOW, end decision with ALLOW
3. else DENY
### AIM policies & S3 Bucket Policies
- IAM policies area attached to group,users,roles
- S3 bucket policies are attached to buckets
- When evaluating if an IAM Principal can perform an operation X on bucket, the union of its assigned IAM Policies and s3 BUcket POlicies will be evaluated.
### IAM PassRole
- iam:passrole give a iam role to aws service, on setup
- based on trust allows, trusted entities on pipeline, trusted relationships
### IAM Best Practices – General
- Never use Root Credentials, enable MFA for Root Account
- Grant Least Privilege
- Each Group / User / Role should only have the minimum level of permission it
needs
- Never grant a policy with “*” access to a service
- Monitor API calls made by a user in CloudTrail (especially Denied ones)
- Never ever ever store IAM key credentials on any machine but a
personal computer or on-premise server
- On premise server best practice is to call STS to obtain temporary
security credentials
### IAM Best Practices – IAM Roles
- EC2 machines should have their own roles
- Lambda functions should have their own roles
- ECS Tasks should have their own roles
(ECS_ENABLE_TASK_IAM_ROLE=true)
- CodeBuild should have its own service role
- Create a least-privileged role for any service that requires it
- Create a role per application / lambda function (do not reuse roles)
### IAM Best Practices – Cross Account Access
- Define an IAM Role for another
account to access
- Define which accounts can access
this IAM Role
- Use AWS STS (Security Token
Service) to retrieve credentials and
impersonate the IAM Role you
have access to (AssumeRole API)
- Temporary credentials can be valid
between 15 minutes to 1 hour 
### Dynamic Policies
- Policies decided in runtime, like per aws account username
### Inline vs Managed Policies
- AWS Managed Policy
• Maintained by AWS
• Good for power users and administrators
• Updated in case of new services / new APIs
- Customer Managed Policy
• Best Practice, re-usable, can be applied to many principals
• Version Controlled + rollback, central change management
- Inline
• Strict one-to-one relationship between policy and principal
• Policy is deleted if you delete the IAM principal
### AWS Directory Services
- AWS Managed Microsoft AD
• Create your own AD in AWS, manage users
locally, supports MFA
• Establish “trust” connections with your onpremise
AD
- AD Connector
• Directory Gateway (proxy) to redirect to onpremise
AD
• Users are managed on the on-premise AD
• just a proxy
- Simple AD
• AD-compatible managed directory on AWS
• Cannot be joined with on-premise AD

### Questions
- We need to gain access to a Role in another AWS account. How is it done?: We should use the STS service to gain temporary credentials
- An EC2 instance has an IAM instance role attached to it, providing it read and write access to the S3 bucket "my_bucket". You have tested the IAM instance role and both reads and writes are working. You then remove the IAM role from the EC2 instance and test both read and write again. Writes stopped working but Reads are still working. What is the likely cause of this behavior?:The S3 bucket policy authorizes reads. When evaluating an IAM policy of an EC2 instance doing actions on S3, the union of both the IAM policy of the EC2 instance and the bucket policy of the S3 bucket are taken into account.
- What's this IAM policy allowing you to do?:
Allowing you to assign IAM Roles to EC2 if they start with "RDS-"
```
{
    "Version": "2012-10-17",
    "Id": "Secret Policy",
    "Statement": [
        {
            "Sid": "EC2",
            "Effect": "Allow",
            "Action": "ec2:*",
            "Resource": "*"
        },
        {
            "Sid": "Passrole",
            "Effect": "Allow",
            "Action": [
                "iam:PassRole"
            ],
            "Resource": "arn:aws:iam:::role/RDS-*"
        }
    ]
}:  
```
- Your AWS account is now growing to 200 users and you would like to provide each of these users a personal space in the S3 bucket "my_company_space" with the prefix /home/<username>, where they have read/write access. How can you do this efficiently?:Create one customer-managed policy with dynamic variables and attach it to a group of all users
