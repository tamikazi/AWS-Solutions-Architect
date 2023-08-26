---
aliases:
- Storage Gateway
- DataSync
- Kinesis
---
# Storage Gateway
- bridging on-premises and cloud data
- used for:
	- disaster recovery
	- backup and restore
	- tiered storage
	- on premise cache and low latency file access
![[Pasted image 20230825193259.png]]
## S3
- most recently used data is cached in the file gateway
- does not support [[Scalable storage solutions|S3]] Glacier directly, can use Lifecycle Policy to transition
- use of [[Secure access to AWS resources|IAM]] Roles for each file gateway
- uses NFS and SMB protocol
![[Pasted image 20230825192518.png]]
## FSx
- native access to [[Storage solutions|FSx]] for Windows
- local cache for infrequently accessed data
- useful for group file shares and home directories
![[Pasted image 20230825192753.png]]
## Volume
- block storage using iSCSI protocol backed by [[Scalable storage solutions|S3]]
- backed by [[Scalable storage solutions|EBS]] snapshots which can help restore on premise volumes
### Cached Volumes
- low latency access to most recent data
### Stored Volumes
- entire dataset is on premise, scheduled backups to S3
![[Pasted image 20230825193022.png]]
## Tape
- backup of physical tapes
# DataSync
- move large amounts of data to and from
	- on premise to AWS - needs agent
	- AWS to AWS - no agent
- can sync to [[Scalable storage solutions|S3]], [[Scalable storage solutions|EFS]], [[Storage solutions|FSx]]
- can be scheduled
- file permissions and metadata are preserved
- one agent can use 10GBPS
![[Pasted image 20230825193941.png]]
![[Pasted image 20230825194009.png]]
# Kinesis
- collect, process and analyze streaming data in real time
## Streams
- capture, process, and store data streams
![[Pasted image 20230825201427.png]]
- retention between 1 - 365 days
- able to reprocess data
- can't be deleted once inserted
- data that shares the same partition goes to the same shard
- Producers: SDK, Kinesis Producer Library, Kinesis Agent
- Consumers: Lambda, Firehose, Analytics
### Capacity Modes
#### Provisioned
- choose number of shards provisioned, scale manually or using API
- each shard gets 1mbps in and 2mbps out
- pay per shard provisioned
#### On-demand
- default 4mbps
- scales automatically based on observed throughput peak during last 30 days
- pay per stream per hour and data in/out per GB
### Security
- [[Secure access to AWS resources|IAM]] policies
- in flight encryption HTTPS
- at rest KMS
- VPC endpoints available
- monitor API calls using CloudTrails
![[Pasted image 20230825202020.png]]

## Firehose
- load data streams into AWS data stores
![[Pasted image 20230825202051.png]]
- fully managed and serverless
	- Redshift/[[Scalable storage solutions|S3]]/OpenSearch
	- 3rd party
	- any HTTP endpoint
- pay for data going through
- near real time
	- min 1MB at a time
- supports data transformation using Lambda
- can send failed or all data to a backup using [[Scalable storage solutions|S3]]
![[Pasted image 20230825202333.png]]
## Analytics
- analyze data streams with SQL or Apache Flink
## Video Streams
- capture, process and store video streams
## Ordering Data
- send partition key which will reference which shard the data will go into
## Kinesis vs. SQS ordering
- Let’s assume 100 trucks, 5 kinesis shards, 1 SQS FIFO
- Kinesis Data Streams:
	- On average you’ll have 20 trucks per shard
	- Trucks will have their data ordered within each shard
	- The maximum amount of consumers in parallel we can have is 5
	- Can receive up to 5 MB/s of data
- SQS FIFO
	- one SQS FIFO queue
	- 100 Group ID
	- up to 100 Consumers (due to the 100 Group ID)
	- up to 300 messages per second (or 3000 if using batching)