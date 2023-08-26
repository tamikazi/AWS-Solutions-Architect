---
aliases:
- EC2
- ENI
- AMI
- CloudFront
---
# EC2: Elastic Compute Cloud
- infrastructure as a service
- Capabilities:
	- renting virtual machines (EC2)
	- storing data on virtual drives (EBS)
	- distributing load across machines (ELB)
	- scaling the services using an auto-scaling group (ASG)
- Utilizes [[Secure workloads and applications|Security Groups]]
- has various [[Cost-optimized database solutions|Purchasing Options]]

## Sizing and Configuration
- OS: Linux, Windows, Mac
- CPU
- RAM
- Storage
	- Network-attached (EBS & EFS)
	- hardware (EC2 Instance Store)
- Network card: speed of card, Public IP
- Security Groups
- Bootstrap script (configure at first launch): EC2 User Data
## Instance Types
### Naming Convention
![[Pasted image 20230817200031.png]]
### General Purpose
- good for web servers or code repositories
- balance between compute, memory and networking
### Compute Optimized
- used for compute-intensive tasks that require high performance processors
	- batch processing workloads
	- media transcoding
	- high performance web servers
	- scientific modeling and machine learning
	- dedicated gaming servers
### Memory Optimized
- fast performance for workloads that process large data sets in memory
	- high performance, relational/ non-relational databases
	- distributed web scale cache stores
	- in-memory databases optimized for BI (business intelligence)
	- apps performing real-time processing of big unstructured data
### Storage Optimized
- good for storage intensive tasks that require high, sequential read and write access to large data sets on local storage
	- high frequency online transaction processing (OLTP) systems
	- relational and NoSQL databases
	- cache for in-memory databases (ex. **Redis**)
	- data warehousing applications
	- distributed file systems

![[Pasted image 20230817201601.png]]

## Placement Groups
- control over instance placement strategy
### Cluster
- clusters instances into a low-latency group in a single [[Secure access to AWS resources|AZ]]
![[Pasted image 20230817221625.png]]

- great network but if rack fails, all instances fail
- recommended for **Big Data job that needs to be completed fast** and **apps that need extremely low latency and high network output**
### Spread
- spreads instances across underlying hardware (max 7 per group per [[Secure access to AWS resources|AZ]])
![[Pasted image 20230818123443.png]]
- Pros:
	- span across many AZ
	- reduced risk of simultaneous failure
	- instances are on different hardware
- Cons:
	- limited to 7 instances per AZ
- recommended for apps that need **max high availability** and **critical jobs where each instance must be isolated from failure from each other**
### Partition
- spreads instances across many different partitions (rely on different set of stacks) withing an AZ
- scales to hundreds of instances per group (Hadoop, Cassandra, Kafka)
![[Pasted image 20230818123738.png]]
- combination of cluster and spread

## Elastic Network Interface (ENI)
- logical component in a [[Technical Terms|VPC]] that represents a virtual network card
- Attributes:
	- primary private IPv4 and 1+ secondary IPv4
	- one [[Technical Terms|Elastic IP]] per private IP
	- one public IPv4
	- 1+ security groups
	- one MAC address
- can create independently and attach them on the fly on EC2 instances for failover
![[Pasted image 20230818124906.png]]


## Amazon Machine Image (AMI)
- customization of an EC2 instance
	- add your own software, configuration, OS, etc.
	- faster boot since all software is pre packaged
- built for a specific region but can be copied to different regions
- Process:
	- start EC2 instance and customize it
	- stop instance (data integrity)
	- build AMI (also creates [[Scalable storage solutions|EBS]] snapshots)
	- launch instances from other AMIs
# CloudFront
- content delivery network (CDN)
- improves read performance, content is cached at the edge
- 216 point of presence globally (edge locations)
- DDoS protection
- files are cached for a TTL (maybe a day)
- great for static content that must be available everywhere
![[Pasted image 20230825173830.png]]
![[Pasted image 20230825184410.png]]
## Geo Restriction
- can have allow and block lists based on country
## Price Classes
- **All**: all regions - best performance
- **200**: most regions, excludes most expensive regions
- **100**: only the least expensive regions
## Cache Invalidation
- can force entire or partial cache refresh (bypassing TTL)
![[Pasted image 20230825184821.png]]
# Global Accelerator
- 2 [[Technical Terms|Anycast IP]] are created for your app
- send traffic directly to edge locations and then directly to your app
![[Pasted image 20230825185204.png]]
- works with [[Technical Terms|Elastic IP]], EC2, [[Scalable and loosely coupled architectures|ALB]], [[Scalable and loosely coupled architectures|NLB]], public or private
- consistence performance
- health checks
- security
## Functions
- lightweight functions written in JavaScript
- for high scale, latency sensitive CDN customizations
- millions of request per second
- native to CloudFront