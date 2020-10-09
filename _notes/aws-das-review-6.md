---
title: AWS DAS Security 
layout: post
tags: [aws, das, encryption, sts]
date: 2020-10-01
--- 
### Encryption in flight
- Why encryption?
- Encryption in flight (SSL)
- Data is encrypted before sending and decrypted after receiving
- SSL certificates help with encryption (HTTPS)
- Encryption in flight ensures no MITM (man in the middle attack) can happen
- Server side encryption at rest
- Data is encrypted after being received by the server
- Data is decrypted before being sent
- It is stored in an encrypted form thanks to a key (usually a data key)
- The encryption / decryption keys must be managed somewhere and the server must have access to it
### Client encryption
- Data is encrypted by the client and never decrypted by the server
- Data will be decrypted by a receiving client
- The server should not be able to decrypt the data
- Could leverage Envelope Encryption
### S3 Encryption for Objects (Reminder)
 There are 4 methods of encrypting objects in S3
- SSE-S3: encrypts S3 objects using keys handled & managed by AWS
- SSE-KMS: leverage AWS Key Management Service to manage encryption keys
- SSE-C: when you want to manage your own encryption keys
- Client Side Encryption
### SSE-S3
- SSE-S3: encryption using keys handled & managed by AWS S3
- Object is encrypted server side
- AES-256 encryption type
- Must set header:   “x-amz-server-side-encryption": "AES256"
### SSE-KMS
- SSE-KMS: encryption using keys handled & managed by KMS
- KMS Advantages: user control + audit trail
- Object is encrypted server side
- Must set header:   “x-amz-server-side-encryption": ”aws:kms"
### SSE-C
- SSE-C: server-side encryption using data keys fully managed by the customer outside of AWS
- Amazon S3 does not store the encryption key you provide
- HTTPS must be used
- Encryption key must provided in HTTP headers, for every HTTP request made
### Client Side Encryption
- Client library such as the Amazon S3 Encryption Client
- Clients must encrypt data themselves before sending to S3
- Clients must decrypt data themselves when retrieving from S3
- Customer fully manages the keys and encryption cycle
### Encryption in transit (SSL)
- AWS S3 exposes:
• HTTP endpoint: non encrypted
• HTTPS endpoint: encryption in flight
- You’re free to use the endpoint you want, but HTTPS is recommended
- HTTPS is mandatory for SSE-C
- Encryption in flight is also called SSL / TLS
### AWS KMS (Key Management Service)
- Anytime you hear “encryption” for an AWS service, it’s most likely KMS
- Easy way to control access to your data, AWS manages keys for us
- Fully integrated with IAM for authorization
- Seamlessly integrated into:
- Amazon EBS: encrypt volumes
- Amazon S3: Server side encryption of objects
- Amazon Redshift: encryption of data
- Amazon RDS: encryption of data
- Amazon SSM: Parameter store
- But you can also use the CLI / SDK

### AWS KMS 101
- Anytime you need to share sensitive information… use KMS
• Database passwords
• Credentials to external service
• Private Key of SSL certificates
- The value in KMS is that the CMK used to encrypt data can never be retrieved by the user, and the CMK can be rotated for extra security
- Never ever store your secrets in plaintext, especially in your code!
- Encrypted secrets can be stored in the code / environment variables
- KMS can only help in encrypting up to 4KB of data per call
- If data > 4 KB, use envelope encryption
- To give access to KMS to someone:
• Make sure the Key Policy allows the user
• Make sure the IAM Policy allows the API calls
### AWS KMS (Key Management Service)
- Able to fully manage the keys & policies:
• Create
• Rotation policies
• Disable
• Enable
- Able to audit key usage (using CloudTrail)
- Three types of Customer Master Keys (CMK):
• AWS Managed Service Default CMK: free
• User Keys created in KMS: $1 / month
• User Keys imported (must be 256-bit symmetric key): $1 / month
+ pay for API call to KMS ($0.03 / 10000 calls)
### Encryption in AWS Services
Requires migration (through Snapshot / Backup):
EBS Volumes
RDS databases
ElastiCache
EFS network file system
In-place encryption:
S3
### CloudHSM
KMS => AWS manages the software for encryption
CloudHSM => AWS provisions encryption hardware
Dedicated Hardware (HSM = Hardware Security Module)
You manage your own encryption keys entirely (not AWS)
HSM device is tamper resistant, FIPS 140-2 Level 3 compliance
CloudHSM clusters are spread across Multi AZ (HA) – must setup
Supports both symmetric and asymmetric encryption (SSL/TLS keys)
No free tier available
Must use the CloudHSM Client Software
Redshift supports CloudHSM for database encryption and key management
Good option to use with SSE-C encryption
### CloudHSM vs KMS
### Security - Kinesis
Kinesis Data Streams
SSL endpoints using the HTTPS protocol to do encryption in flight
AWS KMS provides server-side encryption [Encryption at rest]
For client side-encryption, you must use your own encryption libraries
Supported Interface VPC Endpoints / Private Link – access privately
KCL – must get read / write access to DynamoDB table
Kinesis Data Firehose:
Attach IAM roles so it can deliver to S3 / ES / Redshift / Splunk
Can encrypt the delivery stream with KMS [Server side encryption]
Supported Interface VPC Endpoints / Private Link – access privately
Kinesis Data Analytics
Attach IAM role so it can read from Kinesis Data Streams and reference sources and write to an output destination (example Kinesis Data Firehose)
### Security - SQS
Encryption in flight using the HTTPS endpoint
Server Side Encryption using KMS
IAM policy must allow usage of SQS
SQS queue access policy
Client-side encryption must be implemented manually
VPC Endpoint is provided through an Interface
### Security - AWS IoT
AWS IoT policies:
Attached to X.509 certificates or Cognito Identities
Able to revoke any device at any time
IoT Policies are JSON documents
Can be attached to groups instead of individual Things.
IAM Policies:
Attached to users, group or roles
Used for controlling IoT AWS APIs
Attach roles to Rules Engine so they can perform their actions
### Security – Amazon S3
IAM policies
S3 bucket policies
Access Control Lists (ACLs)
Encryption in flight using HTTPS
Encryption at rest
Server-side encryption: SSE-S3, SSE-KMS, SSE-C
Client-side encryption – such as Amazon S3 Encryption Client
Versioning + MFA Delete
CORS for protecting websites
VPC Endpoint is provided through a Gateway
Glacier – vault lock policies to prevent deletes (WORM)
### Security – DynamoDB
Data is encrypted in transit using TLS (HTTPS)
DynamoDB can be encrypted at rest
KMS encryption for base tables and secondary indexes
Only for new tables
To migrate un-encrypted table, create new table and copy the data
Encryption cannot be disabled once enabled
Access to tables / API / DAX using IAM
DynamoDB Streams do not support encryption
VPC Endpoint is provided through a Gateway
### Security - RDS
VPC provides network isolation
Security Groups control network access to DB Instances
KMS provides encryption at rest
SSL provides encryption in-flight
IAM policies provide protection for the RDS API
IAM authentication is supported by PostgreSQL and MySQL
Must manage user permissions within the database itself
MSSQL Server and Oracle support TDE (Transparent Data Encryption)
### Security - Aurora
(very similar to RDS)
VPC provides network isolation
Security Groups control network access to DB Instances
KMS provides encryption at rest
SSL provides encryption in-flight
IAM authentication is supported by PostgreSQL and MySQL
Must manage user permissions within the database itself
### Security - Lambda
IAM roles attached to each Lambda function
Sources
Targets
KMS encryption for secrets
SSM parameter store for configurations
CloudWatch Logs
Deploy in VPC to access private resources
### Security - Glue
IAM policies for the Glue service
Configure Glue to only access JDBC through SSL
Data Catalog:
Encrypted by KMS
Resource Policies to protect Data Catalog resources (similar to S3 bucket policy)
Connection passwords: Encrypted by KMS
Data written by AWS Glue – Security Configurations:
S3 encryption mode: SSE-S3 or SSE-KMS
CloudWatch encryption mode
Job bookmark encryption mode
### Security - EMR
Using Amazon EC2 key pair for SSH credentials
Attach IAM roles to EC2 instances for:
proper S3 access
for EMRFS requests to S3
DynamoDB scans through Hive
EC2 Security Groups
One for master node
Another one for cluster node (core node or task node)
Encrypts data at-rest: EBS encryption, Open Source HDFS Encryption, LUKS + EMRFS for S3
In-transit encryption: node to node communication, EMRFS, TLS
Data is encrypted before uploading to S3
Kerberos authentication (provide authentication from Active Directory)
Apache Ranger: Centralized Authorization (RBAC – Role Based Access) – setup on external EC2
https://aws.amazon.com/blogs/big-data/best-practices-for-securing-amazon-emr/
### Security – EMR Encryption (security config)
At-rest data encryption for EMRFS:
Encryption in Amazon S3
(SSE-S3, SSE-KMS, Client-Side encryption)
Encryption in Local Disks
At-rest data encryption for local disks:
Open-source HDFS encryption
EC2 Instance Store encryption: NVMe encryption, or LUKS encryption
EBS volumes:
EBS encryption (KMS) – works with root volume LUKS encryption – does not work with root
In-transit encryption:
Node to node communication
For EMRFS traffic between S3 and cluster nodes
TLS encryption
https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-data-encryption-options.html
### Security – ElasticSearch Service
Amazon VPC provides network isolation
ElasticSearch policy to manage security further
Data security by encrypting data at-rest using KMS
Encryption in-transit using SSL
IAM or Cognito based authentication
Amazon Cognito allow end-users to log-in to Kibana through enterprise identity providers such as Microsoft Active Directory using SAML
### Security - Redshift
VPC provides network isolation
Cluster security groups
Encryption in flight using the JDBC driver enabled with SSL
Encryption at rest using KMS or an HSM device (establish a connection)
Supports S3 SSE using default managed key
Use IAM Roles for Redshift
To access other AWS Resources (example S3 or KMS)
Must be referenced in the COPY or UNLOAD command (alternatively paste access key and secret key creds)
### Security - Athena
IAM policies to control access to the service
Data is in S3: IAM policies, bucket policies & ACLs
Encryption of data according to S3 standards: SSE-S3, SSE-
KMS, CSE-KMS
Encryption in transit using TLS between Athena and S3 and
JDBC
Fine grained access using the AWS Glue Catalog
### Security - Quicksight
Standard edition:
IAM users
Email based accounts
Enterprise edition:
Active Directory
Federated Login
Supports MFA (Multi Factor Authentication)
Encryption at rest and in SPICE
Row Level Security to control which users can see which rows
### AWS STS – Security Token Service
- Allows to grant limited and TEMPORARY access to AWS resources.
- Token is valid for up to one hour (must be refreshed)
- Cross Account Access
• Allows users from one AWS account access resources in another
- Federation (Active Directory)
• Provides a non-AWS user with temporary AWS access by linking users Active Directory credentials
• Uses SAML (Security Assertion markup language)
• Allows Single Sign On (SSO) which enables users to log in to AWS console without assigning IAM credentials
- Federation with third party providers / Cognito
• Used mainly in web and mobile applications
• Makes use of Facebook/Google/Amazon etc to federate them
### Cross Account Access
- Define an IAM Role for another account to access
- Define which accounts can access this IAM Role
- Use AWS STS (Security Token Service) to retrieve credentials and impersonate the IAM Role you have access to (AssumeRole API)
- Temporary credentials can be valid between 15 minutes to 1 hour
### Identity Federation
- Federation lets users outside of AWS to assume temporary role for accessing AWS resources.
- These users assume identity provided access role.
- Federation assumes a form of 3rd party authentication
• LDAP
• Microsoft Active Directory (~= SAML)
• Single Sign On
• Open ID
• Cognito
- Using federation, you don’t need to create IAM users (user management is outside of AWS)
### SAML Federation For Enterprises
- To integrate Active Directory / ADFS with AWS (or any SAML 2.0)
- Provides access to AWS Console or CLI (through temporary creds)
-  No need to create an IAM user for each of your employees
### AWS Cognito - Federated Identity Pools For Public Applications
- Goal:Provide direct access to AWS Resources from the Client Side
- How:
• Log in to federated identity provider – or remain anonymous
• Get temporary AWS credentials back from the Federated Identity Pool
• These credentials come with a pre-defined IAM policy stating their permissions
- Example:provide (temporary) access to write to S3 bucket using Facebook Login
### Policies – leveraging AWS variables
- https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_polici es_variables.html
• ${aws:username} : to restrict users to tables / buckets
• ${aws:principaltype} : account, user, federated, or assumed role
• ${aws:PrincipalTag/department} : to restrict using Tags
- https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_polici es_iam-condition-keys.html#condition-keys-wif
• ${aws:FederatedProvider} : which IdP was used for the user (Cognito, Amazon..)
• ${www.amazon.com:user_id} , ${cognito-identity.amazonaws.com:sub} …
• ${saml:sub}, ${sts:ExternalId}
- For S3 - let’s analyze the policies at: https://docs.aws.amazon.com/AmazonS3/latest/dev/example-bucket-policies.html
- For DynamoDB – let’s analyze the policies at: https://docs.aws.amazon.com/amazondynamodb/latest/developergui de/specifying-conditions.html
- Note for RDS – IAM policies don’t help with in-database security, as it’s a proprietary technology and we are responsible for users & authorization
### Policies Advanced
- For S3 - let’s analyze the policies at: https://docs.aws.amazon.com/AmazonS3/latest/dev/example-bucket-policies.html
- For DynamoDB – let’s analyze the policies at: https://docs.aws.amazon.com/amazondynamodb/latest/developergui de/specifying-conditions.html
- Note for RDS – IAM policies don’t help with in-database security, as it’s a proprietary technology and we are responsible for users & authorization
### AWS CloudTrail
- Provides governance, compliance and audit for your AWS Account
- CloudTrail is enabled by default!
- Get an history of events / API calls made within your AWS Account by:
• Console
• SDK
• CLI
• AWS Services
- Can put logs from CloudTrail into CloudWatch Logs
- If a resource is deleted in AWS, look into CloudTrail first!
- CloudTrail shows the past 90 days of activity
- The default UI only shows “Create”, “Modify” or “Delete” events
- CloudTrail Trail:
• Get a detailed list of all the events you choose
• Ability to store these events in S3 for further analysis
• Can be region specific or global
- CloudTrail Logs have SSE-S3 encryption when placed into S3
- Control access to S3 using IAM, Bucket Policy, etc…
### VPC Endpoints
- Endpoints allow you to connect to AWS Services using a private network instead of the public www network
- They scale horizontally and are redundant
- They remove the need of IGW, NAT, etc… to access AWS Services
- Gateway: provisions a target and must be used in a route table. ONLY S3 and DynamoDB
- Interface: provisions an ENI (private IP address) as an entry point (must attach security group) – most AWS services Also called VPC PrivateLink
### Questions
- 

