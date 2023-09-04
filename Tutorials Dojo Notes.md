- [[Scalable and loosely coupled architectures|CloudWatch]] Metrics that need to be manually set up
	- Memory utilization
	- disk swap utilization
	- disk space
	- page file
	- log collection
- Available by default
	- CPU
	- disk reads
	- network packets
- [[Scalable and loosely coupled architectures|Lambda]] functions can be used to handle bursts of traffic and scale faster than [[Elastic compute solutions|ASG]]'s
- [[Database solutions|DynamoDB]] tables are partitioned and the I/O capacity is divided evenly among the partitions, increasing the # of partitions would lead to an increase in performance
- [[Scalable and loosely coupled architectures|API Gateway]] can throttle requests and cache results
- [[Scalable network architectures|CloudFront]] speeds up content delivery which provides better latency but does not do much for the backend
- [[Database solutions|Redshift]] is a relational DB, not NoSQL
- [[Database solutions|RDS]] events only provide operational events such as DB instances events, parameter group events, [[Secure workloads and applications|Security Groups]] events, and snapshot events - you will need a native function to capture data modifying events (INSERT, DELETE, UPDATE)
![[2019-06-07_10-00-40-e7c750751ea701ec7b91cbeeb464f364.png]]
- [[Elastic compute solutions|ASG]] termination logic:
![[ASG-default-policy-evaluation-flowchart.png]]
- Temporary credentials for AWS with SSO capability
	- Setup a Federation proxy or an Identity provider
	- Setup an AWS Security Token Service to generate temporary token
	- Configure an IAM role and an IAM Policy to access the bucket.
- **Artifact** is for compliance purposes
- SSH uses TCP **NOT** UDP
![[2020-01-11_09-55-33-102a3438068e9bb4c45fa670155c2044.png]]
- during [[Database solutions|RDS]] failover, it flips the CNAME to the standby DB
- "too many connections" error happens when a client is connecting to a MySQL DB, RDS Proxy can manage a large number connections by pooling them and reusing connections
- [[Elastic compute solutions|EMR]] is a managed cluster platform that simplifies running big data frameworks
- [[Database solutions|Redshift]] can be used with SQL querying and BI tools
- [[Scalable and loosely coupled architectures|API Gateway]]
	- build RESTful APls and WebSocket APIs optimized for serverless workloads
	- pay only for the API call you receive and amount of data transferred out
- [[Scalable network architectures|Global Accelerator]]
	- provides static anycast IP that server as a fixed entry point to your applications hosted in one or more AWS regions
	- used for non HTTP
- **Elastic Fabric Adapter**: enables you to run apps requiring high levels of inter-node communications at scale on AWS through custom-built OS bypass hardware interface
![[SGNCL-latest.jpg]]
- [[Scalable storage solutions|EBS]]
	- When you create an EBS volume in an Availability Zone, it is automatically replicated within that zone to prevent data loss due to a failure of any single hardware component.
	- After you create a volume, you can attach it to any EC2 instance in the same Availability Zone
	- Amazon EBS Multi-Attach enables you to attach a single Provisioned IOPS SSD (io1) volume to multiple Nitro-based instances that are in the same Availability Zone. However, other EBS types are not supported.
	- An EBS volume is off-instance storage that can persist independently from the life of an instance. You can specify not to terminate the EBS volume when you terminate the EC2 instance during instance creation.
	- EBS volumes support live configuration changes while in production which means that you can modify the volume type, volume size, and IOPS capacity without service interruptions.
	- Amazon EBS encryption uses 256-bit Advanced Encryption Standard algorithms (AES-256)
	- EBS Volumes offer 99.999% SLA.
- **STS** allows you to create and provide trusted users with temporary security credentials to access resources
- with [[Data ingestion and transformation solutions|DataSync]] you can transfer directly to **Glacier** and don't need a lifecycle policy
- Websites hosted on [[Scalable storage solutions|S3]]
	- An S3 bucket that is configured to host a static website. The bucket must have the same name as your domain or subdomain. For example, if you want to use the subdomain portal.tutorialsdojo.com, the name of the bucket must be portal.tutorialsdojo.com.
	- A registered domain name. You can use Route 53 as your domain registrar, or you can use a different registrar.
	- Route 53 as the DNS service for the domain. If you register your domain name by using Route 53, we automatically configure Route 53 as the DNS service for the domain.
- Cannot attach an [[Technical Terms|Elastic IP]] to an [[Scalable and loosely coupled architectures|ALB]], only to an [[Scalable and loosely coupled architectures|NLB]]
- when instances are stopped, data in the instance will be gone and the underlying host can be changed to a new host computer
- [[Scalable and loosely coupled architectures|ELB]] is region bound
- every time you create a subnet, it is automatically in the default VPC routing table
- the allowed block size in VPC is between /16 and /28
- each subnet maps to a single AZ
- [[Scalable and loosely coupled architectures|Lambda]] cannot be a destination for [[Data ingestion and transformation solutions|Kinesis]] firehose
- hibernation cannot be enabled AFTER an instance has already been launched, you need to migrate to a new instance and enable migration from the start
- EC2 Lifecycle states:
	- **`pending`** – The instance is preparing to enter the running state. An instance enters the pending state when it launches for the first time, or when it is restarted after being in the stopped state.
	- **`running`** – The instance is running and ready for use.
	- **`stopping`** – The instance is preparing to be stopped. Take note that you will not billed if it is preparing to stop however, you will still be billed if it is just preparing to hibernate.
	- **`stopped`** – The instance is shut down and cannot be used. The instance can be restarted at any time.
	- **`shutting-down`** – The instance is preparing to be terminated.
	- **`terminated`** – The instance has been permanently deleted and cannot be restarted. Take note that Reserved Instances that applied to terminated instances are **_still billed_** until the end of their term according to their payment option.
- ![[lifecycle-transitions-v2.png]]
- [[Scalable and loosely coupled architectures|Config]] can detect non-compliant tags for AWS resources
- the default limit per region for on demand instances is vCPU based, you can request an limit increase form and resubmit the the failed request once approved
- **Systems Manager** helps manage your apps and infrastructure, shortens time to detect and resolve operational problems, securely at scale
- **Parameter Store** provides secure, hierarchical storage for configuration data and secrets management. 
![[2020-03-29_06-10-21-25e6fb0ac4acbec47824325e0c154564.png]]
- [[Data ingestion and transformation solutions|Glue]]: Job bookmarking works by storing the state of a job’s progress in a persistent data store separate from the job itself. AWS Glue can resume a job from where it left off, even if the job, environment, or underlying data have changed. Job bookmarking is especially useful when dealing with large datasets or long-running jobs, as it helps save time and resources by avoiding unnecessary processing.
- ![[AWS-Instance-Types.png]]
- CloudWatch with RDS:
	- Regular Metrics:
		- CPU utilization
		- database connections
		- freeable memory
	- enhanced metrics
		- RDS processed
		- OS processes
- if you want to process data in close proximity to the users, you need to use lambda@edge since kinesis won't do it at the edge
- you will incur charges for any EBS volumes
- there are no charges for creating and using VPC itself
- **Control Tower** provides a single location to set up new well architected multi account environment and govern workloads with rules for security, operations and internal compliance
	- can prevent deployment of resources that don't conform to the selected policies or detecting non-conformance of provisioned resources
- **Resource Access Manager** allows sharing of resources across accounts or within your organization or OU's, it cannot launch new accounts
- [[Scalable and loosely coupled architectures|Config]] cannot provision accounts, it has rules and remediation actions
![[S3-1024x585.jpg]]
![[s3-object-storage.png]]
- [[Scalable and loosely coupled architectures|CloudTrail]] is primarily used to monitor account activity across AWS resources, API calls
- **Amazon Workspaces** are for virtual desktops in your VPC
- cannot use **EFA** on Windows instances
- **Network Firewall**:
	- Pass traffic through only from known AWS service domains or IP address endpoints, such as Amazon S3.
	- Use custom lists of known bad domains to limit the types of domain names that your applications can access.
	- Perform deep packet inspection on traffic entering or leaving your VPC.
	- Use stateful protocol detection to filter protocols like HTTPS, independent of the port used.
![[2020-01-04_04-04-51-474a73beda1fc209d60e63ed8d226b6a.png]]
- **AppSync** is for GraphQL
- [[Scalable and loosely coupled architectures|CloudTrail]] by default stores event logs encrypted with [[Scalable storage solutions|S3]] SSE
- default message retention period for [[Scalable and loosely coupled architectures|SQS]] is 4 days, can be increased to 14 days
- NFS and SMB protocol means file
- [[Scalable storage solutions|EBS]] volumes can still be used while snapshots are being taken - because they occur asynchronously
- [[Database solutions|Aurora]] failover is automatic
	- same or different AZ: flips the CNAME to a healthy replica and promotes it to primary
	- AZ becomes unavailable: recreates in different AZ
	- no replicas and not serverless: creates new instance in same AZ, on best effort
- **AppSync** pipeline resolvers
	- server-side solution to address the common challenge faced in web applications—aggregating data from multiple database tables. Instead of invoking multiple API calls across different data sources, which can degrade application performance and user experience, AppSync pipeline resolvers enable easy retrieval of data from multiple sources with just a single call. By leveraging Pipeline functions, these resolvers streamline the process of consolidating and presenting data to end-users.
- using SSL to connect to SQL DB instance:
	- Force SSL for all connections — this happens transparently to the client, and the client doesn’t have to do any work to use SSL.
	- Encrypt specific connections — this sets up an SSL connection from a specific client computer, and you must do work on the client to encrypt connections.
	- You can force all connections to your DB instance to use SSL, or you can encrypt connections from specific client computers only. To use SSL from a specific client, you must obtain certificates for the client computer, import certificates on the client computer, and then encrypt the connections from the client computer.
- [[Database solutions|RDS]] multi AZ synchronously replicates in a different AZ but in the same region
- [[Database solutions|ElastiCache]] is the best choice for sub millisecond caching. If running on Memcache, it supports Auto Discovery which has the ability for the client programs to automatically identify all of the nodes in a cache cluster, an to initiate and maintain connections to all of the nodes
- **Lambda function URLs** are HTTP(S) endpoints dedicated to your Lambda function. You can easily create and set up a function URL using the Lambda console or API. Once created, Lambda generates a unique URL endpoint for your use.
- in a **CloudFormation** template, only the Resources section is required, everything else is optional
- **Application Discovery Service** helps plan migration to the cloud by collecting usage and configuration data about on premise server
- **Migration Hub** is s single place to discover your existing servers, plan migration, and track the status of each application migration
- [[Scalable and loosely coupled architectures|NLB]] is capable of doing HTTP health checks
- [[Secure workloads and applications|NACL]] requires you to specify ephemeral ports and you need to allow inbound and outbound since it's stateless
- certificates must be imported into ACM before it can be associated 
- RDP is 3389 - remote desktop access
- warm up period can be defined in step scaling
- **Network Access Analyzer** is for reports on unintended access to resources based on the security and compliance that is set
- **Detective** collects log data from resources to identify the root cause of potential security issues or suspicious activity in your account
- **Traffic Mirroring** can copy network traffic from an [[Scalable network architectures|ENI]]
- CNAME cannot be used created for zone apex
- IP of load balancers can change at any time
- Parquet format is faster for querying

# Flashcards
## File Storage
![[SOAF19-EFS-vs.-S3-vs.-EBS.png]]
## EC2 Volumes
![[SOAF10-SSD-vs-HDD.png]]
![[SOAF9-EBS-Volume-Types.png]]
![[SOAF5-RAID-0-vs.-RAID-1.png]]
## Queues
![[SQS-Standard-vs-FIFO.png]]
## Disaster Recovery
![[CSAPF10-AWS-Database-Disaster-Recovery-Matrix.png]]
## Kinesis Shards
![[DOPF11-Kinesis-Sharding-Strategies.png]]
## Load Balancing
![[ALB-NLB-CLB.png]]
## NAT Gateway vs. Instance
![[SOAF20-NAT-Gateway-vs.-NAT-Instance.png]]
## Database Type Comparison
![[SOAF17-SQL-relational-vs.-NoSQL-nonrelational-databases.png]]
![[SOAF2-Relational-Database-Management-System-RDBMS-vs.-DynamoDB.png]]
## Deployment Comparison
![[SOAF22-Overview-of-CodeDeploy-Compute-Platforms.png]]
## Secondary Index Comparison
![[SOAF13-Global-Secondary-Index-GSI-vs.-Local-Secondary-Index-LSI.png]]
## Multi AZ vs. Read Replicas
![[SOAF3-Multi-AZ-Deployments-vs-Read-Replicas.png]]

