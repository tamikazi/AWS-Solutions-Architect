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
