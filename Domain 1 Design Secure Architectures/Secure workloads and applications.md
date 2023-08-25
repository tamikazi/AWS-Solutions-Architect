---
tags:
- EC2
aliases:
- Security Groups
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