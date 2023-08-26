---
aliases:
- EBS
- EFS
- EC2 Instance Store
- S3
---
# EC2 Instance Store
- high performance hardware disk
- better I/O performance
- lose storage if instance is stopped
- risk of data loss if hardware fails
- good for buffer/cache/scratch data/temporary content
# Elastic Block Store (EBS)
- network drive you can attach to instances while they run
- allows instances to persist data even after termination
	- can control this behaviour
	- by default the root EBS volume is deleted
- can only mount to **one instance at a time**
	- can detach from one and attached to another
	- can attach multiple EBS to an instance
- bound to a specific AZ
	- must create snapshot to move across AZ
- uses network to communicate = latency
- provisioned capacity in GBs and [[Technical Terms|IOPS]]
- only for Linux
- higher price than EBS
## Snapshots
- to make a backup of the volume at a point of time
- not necessary to detach volume to take the snapshot but recommended
- can copy snapshots across AZ
### Archive
- archive tier is 75% cheaper
- takes withing 24-72 hours for restoring the archive
### Recycle Bin
- rules to retain deleted snapshots in case of accidental deletion
- range from 1 day - 1 year
### Fast Snapshot Restore (FSR)
- force full initialization of snapshot to have not latency on the first use (expensive)
## Volume Types
- only gp and io can be used as boot volumes
### GP2/GP3 (SSD)
- general purpose, balances price and performance
- virtual desktops, dev and test environments
- 1GB - 16TB
### IO1/IO2 (SSD)
- highest performance, for critical jobs, low-latency, high throughput
- critical business applications with sustained [[Technical Terms|IOPS]] performance
- 4GB to 16TB
- supports EBS Multi-attach
#### EBS Multi-attach
- attach same EBS volume to multiple EC2 instances in the same AZ
- each instance has full read/write permissions
- higher application availability
- up to **16 instances at a time**
### ST1 (HDD)
- low cost for frequency accessed, throughput-intensive workloads
- Big Data, data warehouses, log processing
- low [[Technical Terms|IOPS]]
### SC1(HDD)
- lowest cost designed for less frequently accessed workloads
- used for where lowest cost is priority
- lowest [[Technical Terms|IOPS]]
## Encryption
- data at rest, in flight and snapshots are all encrypted
- minimal impact on latency
- leverages keys from KMS
- encrypting an unencrypted volume:
	- create EBS snapshot
	- encrypt snapshot (using copy)
	- create new EBS volume from encrypted snapshot
	- attach encrypted volume to original instance
# Elastic File System (EFS)
- managed [[Technical Terms|NFS]] that can be mounted on many instance types
- works for multi-AZ
- highly available, scalable, expensive\
![[Pasted image 20230818140556.png]]
- used for **content management, web serving, data sharing, WordPress**
- uses NFSv4.1 protocol
- uses security group to control access to EFS
- encryption at rest using KMS
- file system scale automatically

## Performance
### Performance Mode
#### General Purpose
- default, latency sensitive use cases (web-server, CMS)
#### Max I/O
- higher latency, throughput, highly available (big data, media processing)
### Throughput Mode
#### Bursting
- 1TB = 50mbps + burst up to 100mbps
#### Provisioned
- set throughput regardless of storage size (1gbps for 1TB of storage)
#### Elastic
- auto scales throughput up or down based on workloads
- up to 3gbps read and 1gbps write
- used for unpredictable workloads
## Storage Classes
- over 90% in cost savings
### Tiers
#### Standard
- for frequently accessed files
#### Infrequent access (EFS-IA)
- cost to retrieve files, low price to store
- enables with Lifecycle policy
### Availability and durability
#### Standard
- multi AZ, great for prod
#### One Zone
- one AZ
- good for dev
- backup enabled by default

# EBS vs EFS
## EBS
![[Pasted image 20230818142440.png]]
## EFS
![[Pasted image 20230818142508.png]]
# Amazon S3
## Uses
- backup and storage
- disaster recovery
- archive
- hybrid cloud storage
- application hosting
- media hosting
- data lakes and big data analytics
- software delivery
- static website
## Buckets
- directories
- must have globally unique name
- defined at region level
## Objects
- these are files that have a Key
	- the full path
	- s3://my-bucket/***my_file.txt***
	- s3://my-bucket/**my_folder1/another_folder/** ***my_file.txt***
	- **prefix** ***object name***
- max object size is 5TB
- max upload size is 5GB
	- if uploading more, must use multi-part upload
## Security
- IAM principal can access S3 object if the permissions allow it OR the resource policy allows it
- Use IAM roles for instance permissions
- public access is blocked by default to prevent data leaks
### User Based
- IAM Policies - which API calls should be allowed for a specific user from IAM
### Resource Based
- Bucket Policies: rules from S3 console - allows cross account
	- JSON based
		- Resources: buckets and objects
		- Effect: allow/deny
		- Actions: set of API to allow or deny
		- Principal: the account or user to apply the policy to
- Object Access Control List - fine grain (can be disabled)
### Encryption
#### Server-Side Encryption (SSE)
##### SSE with S3 managed keys (SSE-S3)
- enabled by default
- encrypts objects using keys handled, managed and owned by AWS
![[Pasted image 20230825170416.png]]
##### SSE with KMS (SSE-KMS)
- uses AWS KMS to manage keys
![[Pasted image 20230825170439.png]]
##### SSE with Customer Provided Keys (SSE-C)
- when you want to manage your own encryption
- AWS does not store encryption key provided
- HTTPS must be used
- key must be provided in HTTP headers for every HTTP request made
![[Pasted image 20230825170505.png]]
#### Client Side Encryption
- client must encrypt prior to sending to S3
- must decrypt themselves
- client fully manages keys and encryption cycle
![[Pasted image 20230825170621.png]]
#### Encryption in transit (SSL/TLS)
- HTTPS recommended and mandatory for SSE-C
## Static Website Hosted
- must allow public access
- The website URL will be (depending on the region)
	- http://bucket-name.s3-website-aws-region.amazonaws.com
	- http://bucket-name.s3-website.aws-region.amazonaws.com
## Versioning
- enabled at **bucket level**
- same key will overwrite the file and change the version
- able to restore versions
- initial version is "null"
## Replication
- must enable versioning first
- buckets can be in different AWS accounts
- copying is asynchronous
- must give proper IAM permissions to S3
- only new objects will be replicated
	- can use **Batch Replication** to replicate existing objects
- can replicate DELETE markers from source to target
- cannot chain replication (ex. 3 buckets)
### Cross-Region Replication (CRR)
- compliance, lower latency access, replication across accounts
### Same-Region Replication (SRR)
- log aggregation, live replication between production and test accounts
## Storage Classes
### Standard - General Purpose
- used for frequently accessed data
- low latency, high throughput
- uses: big data analytics, mobile and gaming apps, content distribution
### Infrequent Access
- less frequently accessed by requires rapid access when needed
- lower cost than Standard
#### Standard IA
- 99.9% availability
- disaster recovery and backup
#### One Zone IA
- highly durable and available
- used for storing secondary backup copies of on-premises data, or data that you can recreate
### Glacier
- low cost, meant for archiving/ back up
#### Instant Retrieval
- millisecond retrieval
- great for data accessed **once per quarter**
- **minimum** storage duration of **90 days**
#### Flexible Retrieval
- **Expedited**: 1 - 5 min retrieval
- **Standard**: 3 - 5 hour retrieval
- **Bulk**: 5 - 12 hour retrieval - free
- **minimum** storage duration of **90 days**
#### Deep Archive
- **Standard**: 12 hour retrieval
- **Bulk**: 48 hour retrieval
- **minimum** storage duration of **180 days**
#### Vault Lock
- Write Once Read Many (WORM)
- create and lock policy for future edits
- good for compliance and data retention
##### Compliance
- versions can't be overwritten or deleted by any user including the root user
- mode can't be changes and retention periods can't be shortened
##### Governance
- most users can't overwrite or delete
- some users can have special permissions to change the retention or delete object
##### Legal Hold
- protect object indefinitely, independent of retention period
### Intelligent Tiering
- small monthly fee and auto-tiering fee
- moves objects automatically based on usage
- no retrieval charges
![[Pasted image 20230825163735.png]]
### Analysis
- helps decide when to transition objects to right storage class
- **only works for** Standard and Standard IA
- updated daily
## Lifecycle Rules
### Transition Actions
- configure object to transition to another storage class
- move to Standard IA after 60 days
- move to Glacier after 6 months
### Expiration Actions
- configure to delete
- access logs can be set to delete after 365 days
- can be used to delete old versions of files
- incomplete multi-part uploads
## Requester Pays
- requester of the bucket pays instead of the default owner
- requester must be authenticated in AWS
## Event Notifications
- any event can be sent to another service
- takes seconds
![[Pasted image 20230825164707.png]]
### EventBridge
- advanced filtering
- multiple destinations
## Baseline Performance
- can achieve at least 3,500 PUT/COPY/POST/DELETE or 5,500 GET/HEAD requests per second **per prefix** in a bucket
	- no limit on number of prefixes
### Transfer Acceleration
- increase transfer speed by transferring file to an AWS edge location which will forward the data to the S3 bucket in the target region
- compatible with multi-part
![[Pasted image 20230825165244.png]]
## Select and Glacier Select
- retrieve less data using SQL by performing **server-side filtering**
- can filter by rows and columns
- less network transfer and CPU cost
![[Pasted image 20230825165543.png]]
## Batch Operations
- encrypt unencrypted objects
- a job consists of a list of objects, the action to perform and optional parameters
- can use S3 Inventory to get object list, use Select to filter objects
![[Pasted image 20230825165758.png]]
## Cross-Origin Resource Sharing (CORS)
- Origin = scheme (protocol) + host (domain) + port
	- example: https://www.example.com (implied port is 443 for HTTPS, 80 for HTTP)
	- Same origin: http://example.com/app1 & http://example.com/app2
	- Different origins: http://www.example.com & http://other.example.com
- The requests won’t be fulfilled unless the other origin allows for the requests, using CORS Headers (example: Access-Control-Allow-Origin)
![[Pasted image 20230825171654.png]]
- if a client makes a cross-origin request, need to enable CORS headers
- can allow specific origin or all origins
![[Pasted image 20230825171901.png]]
## MFA Delete
- required to permanently delete or disable versioning on the bucket
- not required to enable versioning and list deleted versions
- only root user can enable/disable MFA delete
## Access Logs
- any request made will be logged in another S3 bucket
- data can be analyzed
- target logging bucket must be in the same region
## Pre-Signed URLs
- generate using console, CLI or SDK
	- console link expires up to 12 hours
	- CLI longer expiration
- grants permissions of the user that generated the URL for GET and PUT
## Access Points
![[Pasted image 20230825173053.png]]
- has it's own DNS name
- access point policy to manage security at scale
- must define access point to be accessible only from within the VPC
- must create VPC endpoint to access the Access Point
- VPC endpoint policy must allow access tot he target bucket and Access Point
![[Pasted image 20230825173309.png]]
## Object Lambda
- use lambda functions to change object before being retrieved
- need to create S3 Access Point and S3 Object Lambda Access Points
- useful for redacting personally identifiable information for analytics, converting data formats, resizing/watermarking images
![[Pasted image 20230825173525.png]]
## Cross-Region Replication
- must be set up for each region
- files are updated in near real time
- read only
- great for dynamic content that needs to be available at low latency in a few region