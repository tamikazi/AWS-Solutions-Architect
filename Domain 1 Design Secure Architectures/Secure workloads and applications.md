---
tags:
- EC2
- Cognito
aliases:
- Security Groups
- Cognito
- SSM
- WAF
- Shield
- AWS VPC
- IGW
- NACL
- NAT Gateway
- Route Tables
- Direct Connect
---
# Security Groups
- control how traffic is allowed into or out of [[Scalable network architectures|EC2]] instances
- only contain ***allow*** rules
- can reference by IP or security group
- acts as a "firewall"
- Regulates:
	- access to ports
	- authorized IP ranges - IPv4 and IPv6
![[Pasted image 20230817211056.png]]
- can be attached to multiple instances
- locked down to a region/ [[Technical Terms|VPC]]
- good to maintain one separate security group for SSH access
- **time out** = security group issue
- **connection refused** = application error/ not launched
- all inbound traffic is ***blocked*** by default and outbound is ***authorized*** by default
![[Pasted image 20230817211723.png]]
# Cognito
- gives users identity to interact with web or mobile app
## User Pools
- sign in functionality
- integrate with [[Scalable and loosely coupled architectures|API Gateway]] and [[Scalable and loosely coupled architectures|ALB]]
- serverless database
- standard sign in, social media sign in
![[Pasted image 20230826171609.png]]
## Identity Pools (Federated Identity)
- provide AWS credentials to users so they can access AWS resources directly
- integrate with User Pools as identity provider
- 3rd party logins
![[Pasted image 20230826171700.png]]
# SSM Parameter Store
- secure storage for configuration and secrets
- serverless
- security through IAM
- notifications through [[Scalable and loosely coupled architectures|EventBridge]]
- integration with CloudFormation
![[Pasted image 20230827144515.png]]
# Secrets Manager
- capability to force rotation every X days
- automate rotation using [[Scalable and loosely coupled architectures|Lambda]]
- mostly meant for integration with [[Database solutions|RDS]]
- encrypted with [[Appropriate data security controls|KMS]]
## Multi Region Secrets
- replicate across multiple regions
- keeps read replicas in sync with the primary Secret
- ability to promote a read replica Secret to a standalone Secret
# Web Application Firewall (WAF)
- protects web apps from common web exploits (HTTP layer 7)
- Deploy on:
	- [[Scalable and loosely coupled architectures|ALB]]
	- [[Scalable and loosely coupled architectures|API Gateway]]
	- [[Scalable network architectures|CloudFront]]
	- AppSync GraphQL API
	- [[Secure workloads and applications|Cognito]] User Pool
- define Web ACL (Web access control list) rules
	- set up up to 10k IP addresses
	- HTTP headers, HTTP body, or URI strings Protects from common attack - SQL injection and Cross-Site Scripting (XSS)
	- size constraints, geo match
	- rate based rules for DDoS protection
- Web ACL are regional except for [[Scalable network architectures|CloudFront]]
- rule group is reusable set of rules that you can add to a web ACL
![[Pasted image 20230827150803.png]]
# Shield
- DDoS protection
- layer 3 and 4
## Advanced Shield
- optional DDoS migration ($3000 per month)
- protect against more sophisticated attack on
	- [[Scalable network architectures|EC2]]
	- [[Scalable and loosely coupled architectures|ELB]]
	- [[Scalable network architectures|CloudFront]]
	- [[Scalable network architectures|Global Accelerator]]
- protect against higher fees during usage spike
- automatic application layer DDoS migration automatically creates, evaluates, and deploys WAF rules to migrate to layer 7
# Firewall Manager
- manage rules in all accounts of an AWS [[Secure access to AWS resources|Organization]]
# Comparison WAF vs. Firewall Manager vs. Shield
- WAF, Shield and Firewall Manager are used together for comprehensive protection
- Define your Web ACL rules in WAF
- For granular protection of your resources, WAF alone is the correct choice
- If you want to use AWS WAF across accounts, accelerate WAF configuration, automate the protection of new resources, use Firewall Manager with AWS WAF
- Shield Advanced adds additional features on top of AWS WAF, such as dedicated support from the Shield Response Team (SRT) and advanced reporting.
- If you’re prone to frequent DDoS attacks, consider purchasing Shield Advanced
# Best Practices for DDoS Resiliency Edge Location Mitigation
![[Pasted image 20230827153038.png]]
## BP1 CloudFront
- web app delivery at edge
- protect from DDoS common attacks
## BP1 Global Accelerator
- access app at edge
- integration with Shield for DDoS
- helpful if backend in not compatible with [[Scalable network architectures|CloudFront]]
## BP3 Route 53
- domain name resolution at edge
- DDoS protection
## Infrastructure layer defence (BP1, BP3, BP6)
- protect [[Scalable network architectures|EC2]] against high traffic
## EC2 with Auto Scaling (BP7)
- scale in case of sudden traffic surges including a flash crowd or DDoS
## Elastic Load Balancing (BP6)
- [[Scalable and loosely coupled architectures|ELB]] scales with traffic increase and will distribute the traffic to many instances
## Detect and filter malicious requests (BP1, BP2)
- [[Scalable network architectures|CloudFront]] cache static content and serve it from edge locations protecting backend
- WAF used on top of [[Scalable and loosely coupled architectures|ALB]] and [[Scalable network architectures|CloudFront]] to filter and block requests based on request signatures
	- rate based rules can auto block IPs of bad actors
- [[Scalable network architectures|CloudFront]] can block specific geographies
## Shield Advanced (BP1, BP2, BP6)
-auto app layer DDoS mitigation creates, evaluates and deploys WAF rules to mitigate layer 7 attacks
## Obfuscating AWS resources (BP1, BP4, BP6)
- using [[Scalable network architectures|CloudFront]], [[Scalable and loosely coupled architectures|API Gateway]], [[Scalable and loosely coupled architectures|ELB]], to hide backend
## Security Groups and Network ACLs (BP5)
- use [[Secure workloads and applications|Security Groups]] and NACLs to filter traffic based on specific IP at subnet or [[Scalable network architectures|ENI]] level
- [[Technical Terms|Elastic IP]] protected by [[Secure workloads and applications|Shield]] Advanced
## Protecting API endpoints (BP4)
- hide [[Scalable network architectures|EC2]], [[Scalable and loosely coupled architectures|Lambda]] elsewhere
- edge optimized mode or [[Scalable network architectures|CloudFront]] + regional mode
- [[Secure workloads and applications|WAF]] + [[Scalable and loosely coupled architectures|API Gateway]]: burst limits, headers filtering, use API keys
# GuardDuty
- intelligent threat discovery using ML and 3rd party data
- Input Data:
	- [[Scalable and loosely coupled architectures|CloudTrail]] Events Logs
	- VPC Flow logs
	- [[Technical Terms|DNS]] Logs
- can set up [[Scalable and loosely coupled architectures|EventBridge]] rules to be notified in case of findings
	- can target [[Scalable and loosely coupled architectures|Lambda]] or [[Scalable and loosely coupled architectures|SNS]]
- can protect against crypto attacks
# Inspector
- automated security assessments
- assess container images pushed to [[Scalable and loosely coupled architectures|ECR]]
- assesses [[Scalable and loosely coupled architectures|Lambda]] functions for software vulnerabilities in function code and package dependencies and functions as they are deployed
- risk score is associated with all vulnerabilities for prioritization
## EC2 Instances
- uses [[Secure workloads and applications|SSM]] agent
- analyze against unintended network accessibility
- analyze the running OS against known vulnerabilities
![[Pasted image 20230827154751.png]]
# Macie
- ML and pattern matching to discover and protect sensitive data
- PII
# VPC
- all accounts have default VPC
- all instances are launched inside default if no subnet is specified
- get public and private [[Technical Terms|DNS]] names
- max CIDR per VPC is 5
	- min size is /28 (16 IP addresses)
	- max is /16 (65,536 IP addresses)
- VPC CIDR should NOT overlap with other networks
## Classless Inter Domain Routing (CIDR)
- method for allocating IP addresses
- They help to define an IP address range:
	- WW.XX.YY.ZZ/32 => one IP
	- 0.0.0.0/0 => all IPs
	- But we can define:192.168.0.0/26 =>192.168.0.0 – 192.168.0.63 (64 IP addresses)
## Public vs. Private IP
- Private IP can only allow certain values:
	- 10.0.0.0 – 10.255.255.255 (10.0.0.0/8) -> in big networks
	- 172.16.0.0 – 172.31.255.255 (172.16.0.0/12) -> AWS default VPC in that range
	- 192.168.0.0 – 192.168.255.255 (192.168.0.0/16) -> e.g., home networks
- All the rest of the IP addresses on the Internet are Public
## Subnet
- AWS reserves 5 IP addresses (first 4 and last 1) in each subnet
- Example: if CIDR block 10.0.0.0/24, then reserved IP addresses are:
	- 10.0.0.0 – Network Address
	- 10.0.0.1 – reserved by AWS for the VPC router
	- 10.0.0.2 – reserved by AWS for mapping to Amazon-provided DNS
	- 10.0.0.3 – reserved by AWS for future use
	- 10.0.0.255 – Network Broadcast Address. AWS does not support broadcast in a VPC, therefore the address is reserved
## Internet Gateway (IGW)
- allows resources in a VPC to connect to the internet
- scales out and is highly available
- must be created separately from VPC
- 1 VPC per 1 IGW
- need route tables as well
![[Pasted image 20230827160316.png]]
## Bastion Hosts
- can use to [[Technical Terms|SSH]] into private instances
- in the public subnet
- [[Secure workloads and applications|Security Groups]] must allow inbound traffic on port 22 from restricted CIDR, ex. public CIDR of your corporation
![[Pasted image 20230827160755.png]]
## NAT Instance
- network address translation
- allows EC2 instance in private subnets to connect to the internet
![[Pasted image 20230827160832.png]]
## NAT Gateway
- pay per hour for usage and bandwidth
- created in a specific AZ, uses elastic IP
- can't be used by [[Scalable network architectures|EC2]] instance in the same subnet
- requires IGW
- 5GBPS up to 45
- so [[Secure workloads and applications|Security Groups]] to manage
- resilient in 1 AZ but must create multiple in multiple AZ for fault-tolerance
- no cross AZ failover
![[Pasted image 20230827161055.png]]
## Security Groups and NACLs
![[Pasted image 20230827161258.png]]
## Network Access Control List (NACL)
- like firewall which control traffic to and from subnets
- 1 NACL per subnet
- define rules, new NACLs will deny everything
	- do not modify default, create custom NACLs
- great way to block specific IP at the subnet level
![[Pasted image 20230827161518.png]]
## Ephemeral Ports
- for any 2 endpoints, they must use ports
- clients connect to a define port and expect a response from the ephemeral port
![[Pasted image 20230827161719.png]]
![[Pasted image 20230827161747.png]]
## Flow Logs
- capture info going into your interfaces
- data can go to [[Scalable storage solutions|S3]], [[Scalable and loosely coupled architectures|CloudWatch]] Logs and [[Data ingestion and transformation solutions|Kinesis]] Firehose
- query logs using [[Data ingestion and transformation solutions|Athena]] on [[Scalable storage solutions|S3]] or [[Scalable and loosely coupled architectures|CloudWatch]] Insights
![[Pasted image 20230827163311.png]]
### Troubleshoot SG and NACL issues
![[Pasted image 20230827163443.png]]
## Site to Site VPN
![[Pasted image 20230827163522.png]]
### Virtual Private Gateway (VGW)
- VPN concentrator on AWS side of the VPN connection
- must enable Route Propagation in the [[Secure workloads and applications|Route Tables]] that is associated with the subnet
### Customer Gateway (CGW)
- software or physical device on customer side of the VPN connection
- public internet routable IP address
![[Pasted image 20230827163832.png]]
## CloudHub
- secure communication between multiple sites if you have multiple VPN connections
- low cost hub and spoke model for primary or secondary network connectivity
- connect multiple VPN on the same VGW, setup dynamic routing and configure [[Secure workloads and applications|Route Tables]]
![[Pasted image 20230827164111.png]]
## Direct Connect
- dedicated private connection from remote network to VPC
- must be set up between DC and AWS Direct Connect Locations
- need to set up VPG
- access public and private resources on same connection
- uses:
	- increase bandwidth
	- more consistent
	- hybrid environments
- supports IPv4 and IPv6
![[Pasted image 20230827164346.png]]
### Gateway
- to set up one or more VPC in many different regions (same account)
![[Pasted image 20230827164446.png]]
### Connection Types
- takes longer than 1 month to establish a new connection
#### Dedicated
- 1, 10, 100 GBPS capacity
- physical ethernet port dedicated to customer
#### Hosted
- 50, 500 MBPS to 10 GBPS
- capacity can be added or removed on demand
### Encryption
- data in transit is not encrypted but is private
- DC + VPN provides IPsec encrypted private connection
![[Pasted image 20230827164806.png]]
### Resiliency
![[Pasted image 20230827164836.png]]
### Backup
- in case DC fails, Site to Site (expensive) can be set up as a backup
## Traffic Mirroring
- capture and inspect network traffic in your VPC
- route traffic to security appliances
- From [[Scalable network architectures|ENI]] To ENI or [[Scalable and loosely coupled architectures|NLB]]
	- can be in the same VPC or different VPC (using peering)
- capture packets
- uses: content inspection, threat monitoring, troubleshooting
![[Pasted image 20230827170135.png]]
## Egress-only Internet Gateway
- used only for IPv6 (similar to NAT)
- allow instances in VPC outbound connections over IPv6 while preventing internet to initiate connection to instances
- must update [[Secure workloads and applications|Route Tables]]
![[Pasted image 20230827170521.png]]
### Minimizing traffic network costs
![[Pasted image 20230827171013.png]]
## Network Firewall
- protect entire VPC from layer 3 to 7
- inspect all traffic in any direction
- can be managed centrally cross account by AWS Firewall Manager to apply to many VPCs
- allow, drop or alert for the traffic that matches the rules
- active flow inspection to protect against network threats with intrusion prevention
- sends logs of rule matches
![[Pasted image 20230827171353.png]]
