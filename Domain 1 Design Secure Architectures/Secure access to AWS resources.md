---
aliases:
- AZ
- IAM
- Organization
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

## AWS Organizations
- global service
- able to manage multiple AWS accounts
- main account is the management account
- member accounts can only be part of one organization
- consolidated billing across all accounts
- pricing benefits
![[Pasted image 20230827133411.png]]
## Service Control Policies
- IAM policies applied to the OU or accounts to restrict users and roles
- do not apply to the management account (full admin power)
- must have explicit allow (does not allow anything by default like IAM)
![[Pasted image 20230827133605.png]]
## IAM Roles vs. Resource Based Policies
- Cross Account: can use resource based policy OR using a Role as a proxy
- when you assume a role, you give up your original permissions and take the permissions assigned to the role
- when using a resource based policy, the principal doesn't have to give up permissions
## Permission Boundaries
- supported for users and roles NOT groups
- use a managed policy to set the maximum permissions an IAM entity can get
![[Pasted image 20230827134413.png]]
### Uses
- delegate responsibilities with non admins, ex. create new IAM users
- allow developers to self assign policies and manage their own permissions while making sure they can't make themselves an admin
- restrict one specific user instead of a whole account using Organizations and SCP
# IAM Identity Center
- one login for all AWS accounts in all AWS Organizations
- identity providers: built in identity store, 3rd party
![[Pasted image 20230827134948.png]]
## Fine-grained Permissions and Assignments
### Multi Account Permissions
- manage access across AWS accounts in your AWS Org
- Permissions Sets: collection of one or more IAM policies assigned to users and groups to define AWS access
### Application Assignments
- SSO access to many SAML 2.0 business applications
- provide required URLs, certificates and metadata
### Attribute Based Access Control (ABAC)
- permissions based on users' attributes stored in IAM Identity Center Identity Store
- uses: define permissions once then modify AWS access by changing the attributes
![[Pasted image 20230827135621.png]]
### Microsoft Active Directory
- database of objects: user accounts, computers, printers, file shares, security groups
- centralized security management, create account, assign permissions
- objects organized in trees
### Directory Services
#### Managed Microsoft AD
- create your own AD in AWS, manage users locally, supports MFA
#### AD Connector
- directory gateway to redirect on-premise AD, supports MFA
#### Simple AD
- AD compatible managed directory on AWS
- cannot be joined with on premise AD
# Control Tower
- set up and govern a secure a compliant multi account AWS environment
- uses AWS organizations to create accounts
- automate set up of environment in a few clicks
- automate ongoing policy management using guardrails
- detect policy violations and remediate them
- monitor compliance through interactive dashboard
## Guardrails
- **Preventative**: using SCPs (restrict across all your accounts)
- **Detective Guardrail**: using AWS Config (identify untagged resources)
![[Pasted image 20230827140711.png]]
