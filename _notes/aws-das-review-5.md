---
title: AWS DAS Analysis 
layout: post
tags: [aws, das, elastic, kinesis]
date: 2020-10-01
---
## Amazon Elasticsearch Service
- Petabyte-scale analysis and reporting
- The Elastic Stack
- A search engine
- An analysis tool
- A visualization tool (Kibana)
- A data pipeline (Beats / LogStash)
- You can use Kinesis too
- Horizontally scalable
### Elasticsearch concepts
- Documents are the things you’re  searching for. More than text. Every document has ID and type.
- Types  defines the schema and mapping shared by documents that represent the same sort of thing. thing of past.
- Indicies- and index powers search intoo all documents within a collection of types. Contain inverted indices that let you search.
### Elasticsearch applications
- Full-text search
- Log analytics
- Application monitoring
- Security analytics
- Clickstream analytics 
### Amazon Elasticsearch Service
- Fully-managed (but not serverless)
- Scale up or down without downtime
•  But this isn’t automatic
- Pay for what you use
•  Instance-hours, storage, data transfer
- Network isolation
- AWS integration
• S3 buckets (via Lambda to Kinesis)
•  Kinesis Data Streams
•  DynamoDB Streams
•  CloudWatch / CloudTrail
• Zone awareness
### Amazon ES Security
- Resource-based policies
- Identity-based policies
- IP-based policies
- Request signing
- VPC
- Cognito
### Securing Kibana
- Cognito 
- Getting inside a VPC from VPC outside is hard…
Subnet
• Nginx reverse proxy on EC2
forwarding to ES domain
Reverse
proxy
• SSH tunnel for port 5601
• VPC Direct Connect
• VPN On-Premise
### Amazon ES anti-patterns
- OLTP
- No transactions
- RDS or DynamoDB is better
- Ad-hoc data querying
- Athena is better
- Remember Amazon ES is primarily for search & analytics
###  Amazon Athena
- Serverless interactive queries of S3 data
- Interactive query service for S3 (SQL)
- No need to load data, it stays in S3
- Presto under the hood
- Serverless!
- Supports many data formats
- CSV (human readable)
- JSON (human readable)
- ORC (columnar, splittable)
- Parquet (columnar, splittable)
- Avro (splittable)
- Unstructured, semi-structured, or structured
### Athena cost model
- Pay-as-you-go
- $5 per TB scanned
- Successful or cancelled queries count, failed queries do not.
- No charge for DDL (CREATE/ALTER/DROP etc.)
- Save LOTS of money by using columnar formats
- ORC, Parquet
- Save 30-90%, and get better performance
- Glue and S3 have their own charges
### Athena Security
- Access control
- IAM, ACLs, S3 bucket policies
- AmazonAthenaFullAccess / AWSQuicksightAthenaAccess
- Encrypt results at rest in S3 staging directory
- Server-side encryption with S3-managed key (SSE-S3)
- Server-side encryption with KMS key (SSE-KMS)
- Client-side encryption with KMS key (CSE-KMS)
- Cross-account access in S3 bucket policy possible
- Transport Layer Security (TLS) encrypts in-transit (between Athena and S3)
### Athena anti-patterns
- Highly formatted reports / visualization
- That’s what QuickSight is for
- ETL
- Use Glue instead
### Questions Athena
- As a Big Data analyst, you need to query/analyze data from a set of CSV files stored in S3. Which of the following serverless services helps you with this?:AWS Athena
- What are two columnar data formats supported by Athena?:Parquet and ORC
- Your organization is querying JSON data stored in S3 using Athena, and wishes to reduce costs and improve performance with Athena. What steps might you take? Convert the data from JSON to ORC format, and analyze the ORC data with Athena. Using columnar formats such as ORC and Parquet can reduce costs 30-90%, while improving performance at the same time. 
AVRO is not a columnar data format, and isn't the best choice for improving Athena's efficiency.
- When using Athena, you are charged separately for using the AWS Glue Data Catalog. True or False ?: True.
- Which of the following statements is NOT TRUE regarding Athena pricing?: Amazon Athena charges you for failed queries.
## Amazon Redshift
- Fully-managed, petabyte-scale data warehouse
- 10X better performance than other
DW’s
- Via machine learning, massively parallel query execution, columnar storage
- Designed for OLAP, not OLTP
- Cost effective
- SQL, ODBC, JDBC interfaces
- Scale up or down on demand
- Built-in replication & backups
- Monitoring via CloudWatch / CloudTrail 
### Redshift Use-case
- Accelerate analytics workloads
- Unified data warehouse & data lake
- Data warehouse modernization
- Analyze global sales data
- Store historical stock trade data
- Analyze ad impressions & clicks
- Aggregate gaming data
- Analyze social trends
### Redshift architecture
### Redshift Spectrum
- Query exabytes of unstructured data in S3 without loading
- Limitless concurrency
- Horizontal scaling
- Separate storage & compute resources
- Wide variety of data formats
- Support of Gzip and Snappy compression
### Redshift Performance
- Massively Parallel Processing (MPP)
- Columnar Data Storage
- Column Compression
### Redshift Durability
- Replication within cluster
- Backup to S3
- Asynchronously replicated to another region
- Automated snapshots
- Failed drives / nodes automatically replaced
- However – limited to a single availability zone (AZ)
### Scaling Redshift
- Vertical and horizontal scaling on demand
- During scaling:
- A new cluster is created while your old one remains available for reads
- CNAME is flipped to new cluster (a few minutes of downtime)
- Data moved in parallel to new compute nodes
### Redshift Distribution Styles
- AUTO:Redshift figures it out based on size of data
- EVEN:Rows distributed across slices in round-robin
- KEY:Rows distributed based on one column
- ALL:Entire table is copied to every node
### Redshift Sort Keys
- Rows are stored on disk in sorted order based on the column you designate as a sort key
- Like an index
- Makes for fast range queries
- Choosing a sort key
- Recency? Filtering? Joins?
- Single vs. Compound vs Interleaved sort keys
### Importing / Exporting data
- COPY command
- Parallelized; efficient
- From S3, EMR, DynamoDB, remote hosts
- S3 requires a manifest file and IAM role
- UNLOAD command
- Unload from a table into files in S3
- Enhanced VPC routing
### COPY command: More depth
- Use COPY to load large amounts of data from outside of Redshift
- If your data is already in Redshift in another table,
- Use INSERT INTO …SELECT
- Or CREATE TABLE AS
- COPY can decrypt data as it is loaded from S3
- Hardware-accelerated SSL used to keep it fast
- Gzip, lzop, and bzip2 compression supported to speed it up further
- Automatic compression option
- Analyzes data being loaded and figures out optimal compression scheme for storing it
- Special case: narrow tables (lots of rows, few columns)
- Load with a single COPY transaction if possible
- Otherwise hidden metadata columns consume too much space
- Redshift copy grants for cross-region snapshot copies
- Let’s say you have a KMS-encrypted Redshift cluster and a snapshot of it
- You want to copy that snapshot to another region for backup
- In the destination AWS region:
- Create a KMS key if you don’t have one already
- Specify a unique name for your snapshot copy grant
- Specify the KMS key ID for which you’re creating the copy grant
- In the source AWS region:
- Enable copying of snapshots to the copy grant you just created
### DBLINK
- Connect Redshift to PostgreSQL (possibly in RDS)
• Good way to copy and sync
- PostgreSQL instance data between PostgreSQL and Redshift
### Integration with other services
- S3
• DynamoDB
- EMR / EC2
- Data Pipeline
• Database Migration Service
### Redshift Workload Management (WLM)
- Prioritize short, fast queries vs. long, slow queries
- Query queues
- Via console, CLI, or API
### Concurrency Scaling
- Automatically adds cluster capacity to handle increase in concurrent read queries
- Support virtually unlimited concurrent users & queries
- WLM queues manage which queries are sent to the concurrency scaling cluster
### Automatic Workload Management
- Creates up to 8 queues
- Default 5 queues with even memory allocation
- Large queries (ie big hash joins) -> concurrency lowered
- Small queries (ie inserts, scans, aggregations) -> concurrency raised
- Configuring query queues
- Priority
- Concurrency scaling mode
- User groups
- Query groups
- Query monitoring rules
### Manual Workload Management
- One default queue with concurrency level of 5 (5 queries at once)
- Superuser queue with concurrency level 1
- Define up to 8 queues, up to concurrency level 50
- Each can have defined concurrency scaling mode, concurrency level, user groups, query groups, memory, timeout, query monitoring rules
- Can also enable query queue hopping
- Timed out queries “hop” to next queue to try again
### Short Query Acceleration (SQA)
- Prioritize short-running queries over longer-running ones
- Short queries run in a dedicated space, won’t wait in queue behind long queries
- Can be used in place of WLM queues for short queries
- Works with:
CREATE TABLE AS (CTAS)
Read-only queries (SELECT statements)
Uses machine learning to predict a query’s execution time
Can configure how many seconds is “short”
### Resizing Redshift Clusters
- Elastic resize
- Quickly add or remove nodes of same type
(It *can* change node types, but not without dropping connections – it creates a whole new cluster)
- Cluster is down for a few minutes
- Tries to keep connections open across the downtime
- Limited to doubling or halving for some dc2 and ra3 node types.
- Classic resize
- Change node type and/or number of nodes
- Cluster is read-only for hours to days
- Snapshot, restore, resize
- Used to keep cluster available during a classic resize
- Copy cluster, resize new cluster
### VACUUM command
- Recovers space from deleted rows
- VACUUM FULL
- VACUUM DELETE ONLY
- VACUUM SORT ONLY
- VACUUM REINDEX
### Redshift features for 2020
- RA3 nodes with managed storage
- Enable independent scaling of compute and storage
- Redshift data lake export
- Unload Redshift query to S3 in Apache Parquet format
- Parquet is 2x faster to unload and consumes up to 6X less storage
- Compatible with Redshift Spectrum, Athena, EMR, SageMaker
- Automatically partitioned
### Redshift anti-patterns
- Small data sets
- Use RDS instead
- OLTP
- Use RDS or DynamoDB instead
- Unstructured data
- ETL first with EMR etc.
- BLOB data
- Store references to large binary files in S3, not the files themselves.
### Questions ReadShift
- You are working as Big Data Analyst of a data warehousing company. The company uses RedShift clusters for data analytics. For auditing and compliance purpose, you need to monitor API calls to RedShift instance and also provide secured data.
 Which of the following services helps in this regard ?: CloudTrail logs.
- You are working as a Big Data analyst of a Financial enterprise which has a large data set that needs to have columnar storage to reduce disk IO. It is also required that the data should be queried fast so as to generate reports. Which of the following service is best suited for this scenario?:Redshift
- You are working for a data warehouse company that uses Amazon RedShift cluster. It is required that VPC flow logs is used to monitor all COPY and UNLOAD traffic of the cluster that moves in and out of the VPC. Which of the following helps you in this regard ?:By enabling Enhanced VPC routing on the Amazon Redshift cluster
- You are working for a data warehousing company that has large datasets (20TB of structured data and 20TB of unstructured data). They are planning to host this data in AWS with unstructured data storage on S3. At first they are planning to migrate the data to AWS and use it for basic analytics and are not worried about performance.
 Which of the following options fulfills their requirement?:node type ds2.xlarge. Since they are not worried about performance, storage (ds) is more important than computing power (dc,) and expensive 8xlarge instances aren't necessary.
- Which of the following services allows you to directly run SQL queries against exabytes of unstructured data in Amazon S3?:Redshift Spectrum. RDS cannot query S3 data directly.
### Amazon RDS
- Hosted relational database
- Amazon Aurora
- MySQL
- PostgreSQL
- MariaDB
- Oracle
- SQL Server
- Not for “big data”
- Might appear on exam as an example of what not to use
- Or in the context of migrating from RDS to Redshift etc.
- ACID
#### Amazon Aurora
- MySQL and PostgreSQL – compatible
- Up to 5X faster than MySQL, 3X faster than PostgreSQL
- 1/10 the cost of commercial databases
- Up to 64TB per database instance
- Up to 15 read replicas
- Continuous backup to S3
- Replication across availability zones
- Automatic scaling with Aurora Serverless
##### Aurora Security
- VPC network isolation
- At-rest with KMS
- Data, backup, snapshots, and replicas can be encrypted
- In-transit with SSL