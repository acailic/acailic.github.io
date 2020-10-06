---
title: AWS DAS Visualization
layout: post
tags: [aws, das, visualization]
date: 2020-10-01
---

## Visualization
### AmazonQuickSight Business analytics and visualizations in the cloud
### QuickSight
- Fast, easy, cloud powered business
analytics service
- Allows all employees in an organization
to:
•Build visualizations
•Perform ad hoc analysis
•Quickly get business insights from data
•Anytime, on any device (browsers, mobile)
- Serverless
### QuickSight Data Sources
- Redshift
- Aurora / RDS
- Athena
- EC2 hosted databases
- Files (S3 or on premises)
•Excel
•CSV, TSV
- Common or extended log format
- Data preparation allows limited ETL 
### SPICE
- Data sets are imported into SPICE
• Super fast, Parallel, In memory
Calculation Engine
•Uses columnar storage, in memory,
machine code generation
•Accelerates interactive queries on large
datasets
-Each user gets 10GB of SPICE
-Highly available / durable
-Scales to hundreds of thousands of
users
### QuickSight Use Cases
- Interactive ad hoc exploration / visualization of data
- Dashboards and KPI’s
- Stories
•Guided tours through specific views of an analysis
•Convey key points, thought process, evolution of an analysis
- Analyze / visualize data from:
•Logs in S3
•On premise databases
•AWS (RDS, Redshift, Athena, S3)
•SaaS applications, such as Salesforce
•Any JDBC/ODBC data source
### QuickSightAnti Patterns
-Highly formatted canned reports
•QuickSight is for ad hoc queries,
analysis, and visualization
-ETL
•Use Glue instead, although
QuickSight can do some
transformations
### QuickSightAnti Security
- Multi factor authentication on your
account
- VPC connectivity
• Add QuickSight’s IP address range to
your database security groups
- Row level security
- Private VPC access
• Elastic Network Interface, AWS Direct Connect
### QuickSight User Management
•Users defined via IAM, or
email signup
•Active Directory integration
with QuickSight Enterprise
Edition
### QuickSight Pricing
- Annual subscription
• Standard: $9 / user /month
• Enterprise: $18 / user / month
- Extra SPICE capacity (beyond 10GB)
•$0.25 (standard) $0.38 (enterprise) / GB / user / month
- Month to month
•Standard: $12 / user / month
•Enterprise: $24 / user / month
- Enterprise edition
•Encryption at rest
•Microsoft Active Directory integration
### Quicksight Machine Learning Insights
-ML powered anomaly detection
•Uses Random Cut Forest
•Identify top contributors to significant changes in
metrics
-ML powered forecasting
•Also uses Random Cut Forest
•Detects seasonality and trends
•Excludes outliers and imputes missing values
-Autonarratives
•Adds “story of your data” to your dashboards
-Suggested Insights
•“Insights” tab displays read to use suggested
insights
## QuickSight Visual Types
- AutoGraph
- Bar Charts
• For comparison and
distribution (histograms)
- Line graphs
• For changes over time
-Scatter plots, heat maps
•For correlation
- Pie graphs, tree maps
•For aggregation
- Pivot tables
•For tabular data
- Stories
### Alternative visualization tools
### Questions
- Your manager has asked you to prepare a visual on QuickSight to see trends in how money was spent on entertainment in the office in the past 12 months. What visual will you use?:LineChart
- You wish to publish a visual you've created illustrating trends of work coming into your company, for your employees to view. Which tool in QuickSight would be appropriate?:A dashboard is a way to publish a screen of data to a larger audience.
- You want to build a visualization from the data-set you have imported, but you are unsure what visual to select for the best view. What can you do?:Auto-graph will automatically try to select the most appropriate visualization type for the data you have selected.
- The source data you wish to analyze is not in a clean format. What can you do to ensure your visual in QuickSight looks good, with a minimum of effort?:Select edit / preview data before loading it into analysis, and edit it as needed.
- How can you point QuickSight to fetch from your database stored on the EC2 instance in your VPC ?:Add Amazon QuickSight IP range to the allowed IPs of the hosted DB
 
