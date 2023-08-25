# EC2 Hibernate
- in-memory RAM is preserved
	- RAM is written to a file in the root [[Scalable storage solutions|EBS]] volume (must be encrypted)
	- must be less than 150 GB
- faster instance boot (OS isn't stopped)
- used for **long running process**, **saving RAM**, **services that takes time to initialize**
- available for [[Cost-optimized database solutions|On-Demand]], [[Cost-optimized database solutions|Reserved]], and [[Cost-optimized database solutions|Spot]] instances
- cannot be hibernated for more than **60 days**