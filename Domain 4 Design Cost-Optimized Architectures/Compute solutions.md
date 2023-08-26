---
aliases:
- FSx
- Snowball
- Snowcone
---
# EC2 Hibernate
- in-memory RAM is preserved
	- RAM is written to a file in the root [[Scalable storage solutions|EBS]] volume (must be encrypted)
	- must be less than 150 GB
- faster instance boot (OS isn't stopped)
- used for **long running process**, **saving RAM**, **services that takes time to initialize**
- available for [[Cost-optimized database solutions|On-Demand]], [[Cost-optimized database solutions|Reserved]], and [[Cost-optimized database solutions|Spot]] instances
- cannot be hibernated for more than **60 days**
# Snow Family
- secure and portable devices to collect and process data at the edge, migrate data into and out of AWS
- can run EC2 and Lambda functions
- can deploy 1 and 3 years
- must import data to S3 first then Glacier, cannot do direct
- use OpsHub to manage
	- unlocking and configuring
	- transferring files
	- launching and managing instances
	- monitor device metrics
	- launching services
![[Pasted image 20230825185539.png]]
## Snowball Edge
- physical data transport solution, alternative to moving data over the network
- pay per data transfer job
- block storage and S3 compatible object storage
### Storage Optimized
- 80 TB of HDD
- up to 40 vCPUs 80 GB RAM
### Compute Optimized
- 42 TB of HDD or 28 TB of NVMe
- 104 vCPUs 416 GB RAM
- optional GPU
## Snowcone
- small portable computing anywhere, rugged and secure
- edge computing, data storage, data transfer
- 8 TB of HDD or
- 14 TB of SSD
- can send back to AWS offline or using DataSync
- 2 CPUs, 4GB memory
## Snowmobile
- EB transfer (1,000,000 TB)
- high security
- best if you're transferring more that 10 PB of data
# Lambda
- Easy Pricing:
	- Pay per request and compute time
	- Free tier of 1,000,000 AWS Lambda requests and 400,000 GBs of compute time
- Pay per calls:
	- First 1,000,000 requests are free
	- $0.20 per 1 million requests thereafter ($0.0000002 per request)
- Pay per duration: (in increment of 1 ms)
	- 400,000 GB-seconds of compute time per month for FREE
	- == 400,000 seconds if function is 1GB RAM
	- == 3,200,000 seconds if function is 128 MB RAM
	- After that $1.00 for 600,000 GB-seconds