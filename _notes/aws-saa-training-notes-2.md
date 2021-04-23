---
title: AWS SAA Training notes, day 2
layout: post
tags: [aws, saa, test, training]
date: 2021-05-20
---
### VPC
-  Amazon EC2 and Amazon VPC use the IPv4 addressing protocol. Private IPv4 addresses are not reachable over the Internet, but you can assign a globally-unique public IPv4 address to your instance
- In one region
- patterns: Multi VPC and multi account. MUlti VPC: Single team sigle org. Multi Account: large org and multiple teams.
- 5 VPC per account
- Example: 10.0.0.0/16 = all IPs from 10.0.0.0 to 10.0.255.255
- You specify this set of addresses in the form of a Classless Inter-Domain Routing (CIDR) block—for example, 10.0.0.0/16. This is the primary CIDR block for your VPC. You can assign block sizes of between /28 (16 IP addresses) and /16 (65,536 IP addresses)
-From the AWS documentation:
For example, in a subnet with CIDR block 10.0.0.0/24, the following five IP addresses are reserved: • 10.0.0.0: Network address • 10.0.0.1: Reserved by AWS for the VPC router • 10.0.0.2: Reserved by AWS. The IP address of the DNS server is always the base of the VPC network range plus 2; however, we also reserve the base of each subnet range plus 2. For VPCs with multiple CIDR blocks, the IP address of the DNS server is located in the primary CIDR. For more information, see https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_DHCP_Options. html#AmazonDNS
• 10.0.0.3: Reserved by AWS for future use • 10.0.0.255: Network broadcast

- stop at routes.
### Subnets
- AWS reserves the first four IP addresses and the last IP address in each subnet CIDR block.
### Gateway 
- An internet gateway is a horizontally scaled, redundant, and highly available VPC component that allows communication between instances in your VPC and the internet. 
### Network Security
- By default, a security group includes an outbound rule that allows all outbound traffic. You can remove the rule and add outbound rules that allow specific outbound traffic only. If your security group has no outbound rules, no outbound traffic originating from your instance is allow
- You can create a custom network ACL and associate it with a subnet. By default, each custom network ACL denies all inbound and outbound traffic until you add rules
- Route table, Network ACL, Subnet, SG, 

- VPN, Direct Connect, VPC Peering