---
title: AWS CloudFront
layout: post
tags: [aws, cda]
date: 2020-09-15
---

### CloudFront
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
  
