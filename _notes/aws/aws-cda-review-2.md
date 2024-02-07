---
title: AWS CloudFront
layout: post
tags: [aws, cda, cloudfront]
date: 2020-09-15
---

## CloudFront
- Content Delivery Network CDN, improve read performance as content is cached, Edge cached (as presented globally).
- DDoS protection , intergration with AWS web app firewall, Shield. Can expose external https and can talk to internal https backends
- Origins: S3 bucket - for distributinf files and caching them on the edge. Enhanced security with Origin Access Identity. Can be used as ingres to upload.
Custom origin (HTTP)- ALB, EC2 instance, S3 website(static), any http backend
- Edge location forwards to origin or reads from cache-  like high level picture. 
- For ALB or EC2 instance  allow publib ip of edge locations. Setup of of security groups of network.
- GeoRestriction- Whitelist,Blacklist
- ClodFront vs S3 CrossRegionReplication: 
  -Cloud is global edge network. files are cached for TTL(maybe a day). Great for static for availabile everywhere.
  -S3 is available for each region to replicate. files are updated real time. read only. great for dynamic content with low latency in few regions
  
- DNS needs to propagate changes, so it uses domain name from cloud fron and not s3 url
- https://stackoverflow.com/questions/38735306/aws-cloudfront-redirecting-to-s3-bucket
- OAI allows only cloudfront user to access files.
- To make public files on S3, first update bucket to be possible to make files public.

### Caching
- cache based on headers, sessions, query params. Cache lives at each location. Controled by TTL
- Separate caching of dynamic and static content. 
- part of cache can be invalidated by CreateInvalidation API
### Security
- GeoRestriction
- HTTPS : viewer protocol policy, origin protocol policy
- Signed URL/cookies. adding policy for expiration and ip ranges, trusted signers
- CloudFront Signed Url vs S3 Pre-signed URL: CloudFront signed allow acces to a path no matter the origin. Account wide key-pair, filter by ip,date,expiration. Uses cache.
S3 Pre-signed url - issues request as the person who pre-signed url. uses the iam key of signing iam. limited lifetime.

### Questions
- CloudFront Signed URL are commonly used to distribute paid content through dynamic CloudFront Signed URL generation.
- S3 CRR allows you to replicate the data from one bucket in a region to another bucket in another region
