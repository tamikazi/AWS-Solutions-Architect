---
aliases:
- EBS
- EFS
- EC2 Instance Store
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
### User Based
- IAM Policies - which API calls should be allowed for a specific user from IAM
### Resource Based
- Bucket Policies: rules from S3 console - allows cross account
- Object Access Control List - fine grain (can be disabled)
### Encryption
- can encrypt objects use encryption keys
