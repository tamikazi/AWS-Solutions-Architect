---
aliases:
- RDS
- Aurora
- ElastiCache
- DynamoDB
- DocumentDB
- Redshift
---
# EC2 Instance Store
- high performance hardware disk
- better I/O performance
- lose storage if instance is stopped
- risk of data loss if hardware fails
- good for buffer/cache/scratch data/temporary content

# EBS Volume Types
- only gp and io can be used as boot volumes
## GP2/GP3 (SSD)
- general purpose, balances price and performance
- virtual desktops, dev and test environments
- 1GB - 16TB
## IO1/IO2 (SSD)
- highest performance, for critical jobs, low-latency, high throughput
- critical business applications with sustained [[Technical Terms|IOPS]] performance
- 4GB to 16TB
- supports EBS Multi-attach
### EBS Multi-attach
- attach same EBS volume to multiple EC2 instances in the same AZ
- each instance has full read/write permissions
- higher application availability
- up to **16 instances at a time**
## ST1 (HDD)
- low cost for frequency accessed, throughput-intensive workloads
- Big Data, data warehouses, log processing
- low [[Technical Terms|IOPS]]
## SC1(HDD)
- lowest cost designed for less frequently accessed workloads
- used for where lowest cost is priority
- lowest [[Technical Terms|IOPS]]
## EBS Encryption
- data at rest, in flight and snapshots are all encrypted
- minimal impact on latency
- leverages keys from KMS
- encrypting an unencrypted volume:
	- create EBS snapshot
	- encrypt snapshot (using copy)
	- create new EBS volume from encrypted snapshot
	- attach encrypted volume to original instance

# Relational Database Service (RDS)
- managed DB service that uses SQL as a query language
- create databases in the cloud that are managed by AWS:
	- Postgres
	- MySQL
	- MariaDB
	- Oracle
	- Microsoft SQL Server
	- Aurora (AWS database)
## Managed Services
- automated provisioning, OS patching
- continuous backups and restore to s specific timestamp (Point in Time Restore)
- monitoring dashboards
- read replicas
- multi AZ
- maintenance windows for upgrades
- storage backed by EBS (gp2 or io1)
- **cannot** [[Technical Terms|SSH]] into your instances
## Storage Auto Scaling
- increase storage dynamically
	- free storage is less than 10% of allocated storage
	- low storage lasts at least 5 minutes
	- 6 hours have passed since last modification
- need to set max storage threshold
- useful for **unpredictable workloads**
- supports all RDS database engines
## Read Replicas
![[Pasted image 20230819151006.png]]
- up to 15
- within AZ, cross AZ or cross region
- replication is **ASYNC** so reads are eventually consistent
- can be promoted to their own DB
- apps must update the connection string to leverage this
### Use Cases
- production database that is taking on normal load
- run reporting app for analytics
- run new workload on the replica
- production app is unaffected
- can **only SELECT(=read)**
![[Pasted image 20230819151326.png]]

## Multi AZ (Disaster recovery)
- SYNC replication
- one [[Technical Terms|DNS]] name, automatic app standby
- no manual intervention in apps
- not used for scaling
- must be set up as Multi AZ for Disaster Recovery
![[Pasted image 20230819151815.png]]
## Single to Multi AZ
- 0 downtime
	- snapshot is taken
	- new DB is restored from the snapshot in a new AZ
	- synchronization is established between 2 databases
![[Pasted image 20230819152035.png]]
## Custom
- managed Oracle and Microsoft SQL Server databased with OS and database customization
	- configure settings
	- install patches
	- enable native features
	- access the underlying [[Scalable network architectures|EC2]] instance using [[Technical Terms|SSH]] or SSM Session Manager
- deactivate automation mode to perform customization, remember to take snapshot before
## Backups
- stopped RDS database will still incur costs, if stopping for a long time, create snapshot and restore instead
### Automated Backups
- daily full backup
- able to restore to any point in time
- 1-35 days of retention
### Manual DB Snapshots
- manually triggered and kept as long as you want
# Aurora
- support for Postgres and MySQL
- 5x performance boost over MySQL on RDS and 3x of Postgres
- can have up to 15 replicas
- failover is instant
- 20% more cost but more efficient
## High Availability and Read Scaling
- 6 copies of your data across 3 AZ
	- 4 of 6 needed for writes
	- 3 of 6 needed for reads
	- self-healing, peer-to-peer replication
	- storage stripped across 100s of volumes
- one instance take writes (master)
- automated failover in less than 30s
- master also takes reads
- **cross region support available**
![[Pasted image 20230819153029.png]]

![[Pasted image 20230819153113.png]]

![[Pasted image 20230819153214.png]]
## Custom Endpoints
![[Pasted image 20230819153306.png]]
- define subset of Aurora instances as a custom endpoint to run analytical queries on specific replicas
- reader endpoint not used after defining custom endpoints
## Serverless
![[Pasted image 20230819153508.png]]
- automated DB instantiation and auto-scaling based on actual usage
- good for infrequent, intermittent or unpredictable workloads
- no capacity planning needed
- pay per second can be cost effective
## Multi Master
- **continuous write availability** for write nodes
- every node does R/W
![[Pasted image 20230819153829.png]]
## Global Aurora
### Cross Region Read Replicas
- useful for disaster recovery
### Global Database (recommended)
- 1 primary region (read/write)
- 5 secondary read only regions
	- 16 replicas per secondary region
- decreases latency
- promoting another region takes <1 minute
- cross region replication takes <1 second
## Machine Learning
![[Pasted image 20230819170607.png]]
## Cloning
- create new DB cluster from existing
- faster than snapshot and restore
- uses copy-on-write protocol
	- initially new DB cluster uses same data volume as original DB cluster
	- additional storage is allocated and data is copied to be separated after updates are made to the new DB cluster
- useful to create a "staging" DB from a "production" DB without impacting the production DB
# Restore Options
- from snapshot creates new DB
- RDS
	- create backup of on premise DB
	- store on **S3**
	- restore the backup file onto new RDS instance running MySQL
- Aurora
	- create backup on premise DB using Percona XtraBackup
	- store on **S3**
	- restore onto a new Aurora cluster running MySQL
# Security
- At-rest encryption:
- Database master & replicas encryption using AWS KMS – must be defined as launch time
- If the master is not encrypted, the read replicas cannot be encrypted
- To encrypt an un-encrypted database, go through a DB snapshot & restore as encrypted
- In-flight encryption: TLS-ready by default, use the AWS TLS root certificates client-side
- IAM Authentication: IAM roles to connect to your database (instead of username/pw)
- Security Groups: Control Network access to your RDS / Aurora DB
- No SSH available except on RDS Custom
- Audit Logs can be enabled and sent to CloudWatch Logs for longer retention

# ElastiCache
- to get managed Redis or Memcached
- requires heavy application code changes
![[Pasted image 20230819172415.png]]
- helps relieve load on RDS
- cache must have invalidation strategy to make sure on the most current data is used in there
## User Session Store
- user logs into app
- app writes session data into ElastiCache
- user hits another instance of the app
- instance retrieves the data and user is already logged in
## Redis vs. Memcached
![[Pasted image 20230819172645.png]]
### Redis Use
- gaming leaderboards
- Redis stored sets guarantee both uniqueness and element ordering
	- each time a new element is added it's ranked in real time, then added in correct order
## Cache Security
- supports [[Secure access to AWS resources|IAM]] authentication for Redis
- [[Secure access to AWS resources|IAM]] policies are only used for AWS API-level security
### Redis AUTH
- set password/token when creating Redis cluster
- support [[Technical Terms|SSL]] in flight encryption
### Memcached
- supports SASL-based authentication
## Patterns
### Lazy Loading
- all read data is cached, can become stale
### Write Through
- adds or update data in the cache when written to a DB (no stale data)
### Session Store
- store temporary session data in a cache using TTL features
# DynamoDB
- made of tables
- each table has primary key
- can have infinite items
- max size is 400KB
- Data types supported:
	- Scalar: string, number, binary, Boolean, null
	- Document types: list, map
	- Set types: string set, num set, binary set
- TTL - auto delete items after expiry timestamp
## Capacity Modes
### Provisioned Mode
- specify reads/writes per second
- plan beforehand
- pay for what is provisioned
- able to add autoscaling
### On Demand Mode
- read/write auto scales
- pay per use, more expensive
- good for unpredictable workloads
## Accelerator (DAX)
- fully managed, highly available, in memory cache
- solves read congestion
- default 5 min TTL
## Stream Processing
- ordered stream of item-level modifications (create/delete/update)
- uses:
	- React to changes in real-time (welcome email to users)
	- Real-time usage analytics
	- Insert into derivative tables
	- Implement cross-region replication
	- Invoke AWS Lambda on changes to your DynamoDB table
- 24 hour retention
- limited # of consumers
- can use [[Scalable and loosely coupled architectures|Lambda]] trigger or [[Data ingestion and transformation solutions|Kinesis]] adapter to process
## Global Tables
![[Pasted image 20230826165114.png]]
- accessible with low latency in multiple regions
- must enable Streams as pre-requisite
## Backups for disaster recovery
- continuous backups using PITR
	- optionally enabled for the last 35 days
	- creates new table
- on demand
	- full back up for long term retention until explicitly deleted
	- doesn't affect performance or latency
	- creates new table
## Integration with S3
### Export
- works for any point in time in last 35 days
- doesn't affect read capacity
- perform data analysis
- retain snapshots for auditing
- export in JSON or ION format
### Import
- import CSV, JSON or ION
- doesn't consume write capacity
- creates new table
- import errors logged in CloudWatch logs
# DocumentDB
- for MongoDB
	- used to store, query, and index JSON data
- fully managed, highly available, reps across 3 AZ
- storage auto grows from 10GB to 64TB
# Neptune
- graph database
- think "social network"
# Keyspaces
- for Apache Cassandra
# QLDB
- Quantum Ledger Database
- recording financial transactions
# Timestream
- time series database
# Redshift
- based on PostgreSQL but not used for OLTP
- used for online analytical processing (OLAP)
- 10x better performance than other data warehouses
- columnar storage of data
- pay as you go
- SQL interface
## Cluster
- Leader Node: for query planning, results aggregation
- Compute Node: performing queries, send results to leader
- provision node in advance
- can use reserved instances for cost savings
![[Pasted image 20230826173200.png]]
## Snapshots and DR
- supports multi AZ for some clusters
- snapshots are point in time backups of a cluster stored internally in [[Scalable storage solutions|S3]]
	- only what has changed is saved
- restore snapshot into a new cluster
- has automated and manual
![[Pasted image 20230826173403.png]]
## Spectrum
- query data that is already in S3 without loading it
- query is then submitted to thousands of Spectrum nodes
![[Pasted image 20230826173513.png]]
