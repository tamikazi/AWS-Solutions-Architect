---
aliases:
- Route 53
- DR
- DMS
- Backup
- Discovery Service
---
# Amazon RDS Proxy
- fully managed database proxy for RDS
- allows apps to pool and share DB connections established with the database
- reduces CPU and RAM, minimize open connections (and timeouts)
- serverless, autoscaling, highly available (multi AZ)
- reduced failover time by 66%
- supports RDS and Aurora
- enforce [[Secure access to AWS resources|IAM]] authentication for DB and securely store credentials in AWS Secrets Manager
- can never be publicly accessed (must be from [[Technical Terms|VPC]])
![[Pasted image 20230819172118.png]]
# Route 53
- fully managed and **authoritative** [[Technical Terms|DNS]]
- domain registrar
![[Pasted image 20230819173932.png]]
## Records
- how you want to route traffic for a domain
- Contains:
	- Domain/subdomain Name – e.g., example.com
	- Record Type – supports, A, AAAA, CNAME, NS
	- Value – e.g., 12.34.56.78
	- Routing Policy – how Route 53 responds to queries
	- TTL (time to live) – amount of time the record cached at DNS Resolvers
### TTL
- high = 24hrs
- low = 60s
- except for alias records, TTL is mandatory for each [[Technical Terms|DNS]] record
![[Pasted image 20230819174920.png]]
## Hosted Zones
- container for records that define how to route traffic toa  domain and it's subdomains
- Public Hosted Zones – contains records that specify how to route traffic on the Internet (public domain names)
	- **application1.mypublicdomain.com** 
- Private Hosted Zones – contain records that specify how you route traffic within one or more VPCs (private domain names)
	- **application1.company.internal**
- 0.50/month per hosted zone
![[Pasted image 20230819174537.png]]
## CNAME vs. Alias
### CNAME:
- Points a hostname to any other hostname. (app.mydomain.com => blabla.anything.com)
- ONLY FOR NON ROOT DOMAIN (aka. something.mydomain.com)
### Alias:
- Points a hostname to an AWS Resource (app.mydomain.com => blabla.amazonaws.com)
- Works for ROOT DOMAIN and NON ROOT DOMAIN (aka mydomain.com)
- Free
- Native health check
- extension of [[Technical Terms|DNS]] functionality
- automatically recognizes changes int he resource's IP addresses
- always A/AAAA
- can't set TTL
- can't set for an [[Scalable network architectures|EC2]] [[Technical Terms|DNS]] name
![[Pasted image 20230819175235.png]]
#### Targets
- Elastic Load Balancers
- CloudFront Distributions
- API Gateway
- Elastic Beanstalk environments
- S3 Websites
- [[Technical Terms|VPC]] Interface Endpoints
- Global Accelerator accelerator
- Route 53 record in the same hosted zone
## Routing Policies
- defining how Route 53 responds to DNS queries
### Simple
- route to single resource
- can specify multiple values in the same record, chosen at random
- only specify one resource
- can't be associated with health checks
### Weighted
- control % of traffic that go to each resource
- [[Technical Terms|DNS]] records must have the same name and type
- can be associated with health checks
- used for load balancing between regions, testing new app versions
- assign weight of 0 to a record to stop sending traffic
	- if all are 0, all will be returned equally
### Latency
- redirect based on lowest latency
- can be associated with health checks and has failover capabilities
### Geolocation
- based on user location
- specify continent, country, or US state
- should create default if no hits
- can use health checks
### Geoproximity
- based on location and resources
- able to shift more resources based on the defined bias
- To change the size of the geographic region, specify bias values:
	- To expand (1 to 99) – more traffic to the resource
	- To shrink (-1 to -99) – less traffic to the resource
- must use Route 53 **Traffic Flow**
### IP Based
- clients IP
- provide list of CIDR's
- optimize performance, reduce network costs
### Multi Value
- route traffic to multiple resources
- can use health checks
- up to 8 healthy records are returned per query
- not a substitute for [[Scalable and loosely coupled architectures|ELB]]
![[Pasted image 20230819181316.png]]
## Health Checks
- only for public resources
- automate [[Technical Terms|DNS]] failover
- can monitor endpoint, other health checks and CloudWatch alarms
### Monitor Endpoint
![[Pasted image 20230819180151.png]]
### Calculated
- combine using **OR, AND or NOT**
- can monitor up to 256 Child health checks
- useful for performing maintenance to your website without causing all the health checks to fail
![[Pasted image 20230819180351.png]]
### Private Hosted Zones
- route 53 health checkers are outside the VPC
- can't access private endpoints
- can create CloudWatch Metric, associate with Alarm, then create health check for the alarm itself
![[Pasted image 20230819180638.png]]
# Disaster Recovery (DR)
## RPO and RTO
![[Pasted image 20230827171829.png]]
## Strategies
![[Pasted image 20230827171856.png]]
### Backup and Restore
- high RPO
![[Pasted image 20230827171946.png]]
### Pilot Light
- small version of app is always running in the cloud
- useful for the critical core
- faster than backup and restore as critical systems are already up
![[Pasted image 20230827172111.png]]
### Warm Standby
- full system is up but at minimum size
- can scale to production load
![[Pasted image 20230827172152.png]]
### Multi Site / Hot Site
- very low RTO, very expensive
- full production scale is running on AWS
![[Pasted image 20230827172241.png]]
# Database Migration Service (DMS)
- migrate DB to AWS
- source DB remains available during migration
- continuous data replication using CDC
- MUST create EC2 to perform the task
![[Pasted image 20230827172522.png]]
## Sources and Targets
### Sources
- On-Premises and EC2 instances databases: Oracle, MS SQL Server, MySQL, MariaDB, PostgreSQL, MongoDB, SAP, DB2
- Azure: Azure SQL Database
- Amazon RDS: all including Aurora
- Amazon S3
- DocumentDB
### Targets
- On-Premises and EC2 instances databases: Oracle, MS SQL Server, MySQL, MariaDB, PostgreSQL, SAP
- Amazon RDS
- Redshift, DynamoDB, S3
- OpenSearch Service
- Kinesis Data Streams
- Apache Kafka
- DocumentDB & Amazon Neptune
- Redis & Babelfish
## Schema Conversion Tool
- convert DB schema from one engine to another
- Example OLTP: (SQL Server or Oracle) to MySQL, PostgreSQL, Aurora
- Example OLAP: (Teradata or Oracle) to Amazon Redshift
![[Pasted image 20230827172821.png]]
## Multi AZ
- DMS provisions and maintains a synchronously stand replica in a different AZ
- provides data redundancy
- eliminates I/O freezes
- minimizes latency spikes
## RDS and Aurora MySQL Migrations
- use DMS if both databases are up and running
### RDS to Aurora
#### Option 1
- DB snapshot from [[Database solutions|RDS]] restored as [[Database solutions|Aurora]]
#### Option 2
- create [[Database solutions|Aurora]] read replica from [[Database solutions|RDS]] and when replication lag is 0, promote as its own DB cluster
### External MySQL to Aurora
#### Option 1
- use Percona XtraBackup to create file backup in [[Scalable storage solutions|S3]]
- create [[Database solutions|Aurora]] DB from [[Scalable storage solutions|S3]]
#### Option 2
- create [[Database solutions|Aurora]] DB
- use mysqldump utility to migrate into [[Database solutions|Aurora]] (slower than option 1)
## RDS and Aurora PostgreSQL Migrations
### RDS to Aurora
#### Option 1
- DB Snapshots from RDS restored as Aurora DB
#### Option 2
- Create an Aurora Read Replica from your RDS, and when the replication lag is 0, promote it as its own DB cluster (can take time and cost $)
### External PostgreSQL to Aurora
- Create a backup and put it in Amazon S3
- Import it using the aws_s3 Aurora extension
- Use DMS if both databases are up and running
# Backup
- centrally manage and automate backups across services
- Supported Services:
	- [[Scalable network architectures|EC2]]/[[Scalable storage solutions|EBS]]
	- [[Scalable storage solutions|S3]]
	- [[Database solutions|RDS]]/[[Database solutions|Aurora]]/[[Database solutions|DynamoDB]]
	- [[Database solutions|DocumentDB]]/Neptune
	- [[Scalable storage solutions|EFS]]/[[Storage solutions|FSx]]
	- [[Data ingestion and transformation solutions|Storage Gateway]]
- supports cross region and account backups
- on demand and scheduled
- backup policies known as Backup Plans
	- frequency
	- window
	- transition to cold storage
	- retention period
- sent to [[Scalable storage solutions|S3]]
## Vault Lock
- WORM state for all backups
- cannot be deleted
# Discovery Service
- plan migration projects by gathering information about on premises data centers
## Agentless
- VM inventory, configuration and performance history
## Agent based
- system configuration, performance, running processes and details of network connections between systems
# Application Migration Service
- Lift-and-shift (rehost) solution which simplify migrating applications to AWS
- Converts your physical, virtual, and cloud-based servers to run natively on AWS
- Supports wide range of platforms, Operating Systems, and databases
- Minimal downtime, reduced costs
![[Pasted image 20230827174517.png]]
# VMware Cloud
- migrate your VMware vSphere based workloads to AWS
# Transferring Large Amounts of Data
Example: transfer 200 TB of data in the cloud. We have a 100 Mbps internet
connection.
## Over the internet / Site-to-Site VPN
- Immediate to setup
- Will take 200(TB)x1000(GB)x1000(MB)x8(Mb)/100 Mbps = 16,000,000s = 185d
## Over direct connect 1Gbps
- Long for the one-time setup (over a month)
- Will take 200(TB)x1000(GB)x8(Gb)/1 Gbps = 1,600,000s = 18.5d
## Over Snowball
- Will take 2 to 3 snowballs in parallel
- Takes about 1 week for the end-to-end transfer
- Can be combined with DMS

- For on-going replication / transfers: Site-to-Site VPN or DX with DMS or
DataSync