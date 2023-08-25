---
aliases:
- AZ
- IAM
---
# AWS Regions
- A region is a cluster of data centers
- Most AWS services are region-scoped
	- Compliance with data governance and legal requirements: data never leaves a region without your permission
	- Proximity: affects latency
	- Availability of services: not all services are available in all regions
	- Pricing: varies region to region

# AWS Availability Zones (AZ)
- Groups of data centers that are region specified
- Minimum 3, maximum 6 per region
- One or more discrete data centers that are isolated and connected with high bandwidth and ultra-low latency networking
## Global Services
- Identity and Access Management (**IAM**)
- **Route 53**: [[Technical Terms|DNS]] Service
- **Cloud Front**: Content Delivery Network
- **WAF**: Web Application Firewall
## Region Scoped
- [[Scalable network architectures|EC2]]: Infrastructure as a service
- **Elastic Beanstalk**: Platform as a service
	- developer centric view of deploying an app
	- managed service
	- free but pay for underlying EC2 instances
- **Lambda**: Function as a service
- **Rekognition**: Software as a service

# IAM
- Identity and Access Management
- **Root account**: created default, shouldn't be used or shared
- **Users**: people within your organization
	- users do not need to belong to a group but that's bad practice
- **Groups**: can only contain users, not other groups
- ![[Pasted image 20230817161531.png]]
## Permissions
- users or groups can be assigned policies (permissions) via [[Technical Terms|JSON]] documents
- apply **least privilege principle**: don't give a user more permissions than needed
- a group's policies will be inherited to its users
### Structure
- **Version**: language version
- **Id**: identifier for the policy *(optional)
- **Statement**: one or more individual statements
	- ***Sid***: identifier for the statement *(optional)
	- ***Effect***: whether the statement allows or denies
	- ***Principal***: account/user/role to which this policy is applied to
	- ***Action***: list of actions this policy allows
	- ***Resource***: list of resources to which the actions applied to
	- ***Condition***: condition for when the policy is in effect *(optional)*
## Password Policy
- set minimum password length
- require specific characters
- allow all IAM users to change their own passwords
- enable password expiration
- prevent password reuse
### MFA
- always protect root users and IAM users
- can use virtual or physical MFA
## Accessing AWS
- AWS Management Console: protected by password + MFA
- AWS Command Line Interface: protected by access keys
	- interact with AWS services through command line shell
	- develop scripts and manage resources
- AWS Software Developer Kit ([[Technical Terms|SDK]]): protected by access keys
	- access keys are generated through AWS Console
	- users manage their own keys
	- ***Access Key ID***: username
	- ***Secret Access Key***: password
	- language specific [[Technical Terms|API]]s
## Roles for Services
- some AWS service will need to perform actions on your behalf
- assign permissions to AWS with IAM roles
- Common roles:
	- EC2 Instance Roles
	- Lambda Function Roles
	- Roles for CloudFormation
## Security Tools
- **Credentials Report**: lists all your account's users and the status of their credentials
- **IAM Access Advisor**: shows the service permissions granted to a users and last accessed information
	- can use this to revise policies
## Guidelines and Best Practices
- don't use root account other than for account set up
- one user per AWS account
- assign all users to a group
- create strong password policy + MFA
- use access keys for programmatic access
- audit permissions using Security Tools

