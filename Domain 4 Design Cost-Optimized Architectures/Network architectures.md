---
aliases:
- VPC Peering
- Transit Gateway
---
# VPC Peering
- privately connect 2 [[Technical Terms|VPC]] using AWS network
- behave as if they are in the same network
- must not have overlapping CIDRs
- not transitive
- must update [[Secure workloads and applications|Route Tables]] in EACH VPC's subnet
- can create in different accounts/regions
![[Pasted image 20230827162156.png]]
![[Pasted image 20230827162257.png]]
# VPC Endpoints
- allows connection to AWS services using private network instead of public internet
- scale out
- remove need for [[Secure workloads and applications|IGW]], [[Secure workloads and applications|NAT Gateway]]
- if there are issues check DNS setting resolution in your VPC and [[Secure workloads and applications|Route Tables]]
![[Pasted image 20230827162529.png]]
## Types
### Interface Endpoint
- provisions an [[Scalable network architectures|ENI]] (private IP) as an entry point (must attach [[Secure workloads and applications|Security Groups]])
- supports most AWS services
- $ per hour and per GB of data
- preferred for from on premise
### Gateway Endpoints
- provisions a gateway and MUST be used as a target in a [[Secure workloads and applications|Route Tables]] (no [[Secure workloads and applications|Security Groups]])
- supports only [[Scalable storage solutions|S3]] and [[Database solutions|DynamoDB]]
- free
- preferred in most cases
![[Pasted image 20230827162838.png]]
### Lambda in VPC accessing DynamoDB
- deploy VPC gateway endpoint for [[Database solutions|DynamoDB]]
- change the [[Secure workloads and applications|Route Tables]]
![[Pasted image 20230827163100.png]]
# Transit Gateway
- For having transitive peering between thousands of VPC and on-premises, hub-and-spoke (star) connection
- regional but can work cross region
- share cross account
- peer across regions
- [[Secure workloads and applications|Route Tables]]: limit which VPC can talk with other VPC
- works with [[Secure workloads and applications|Direct Connect]] Gateway and VPN connections
- support IP multicast
![[Pasted image 20230827165349.png]]
## Site to Site VPN ECMP
- Equal cost multi path routing
- allow to forward a packet over multiple best path
- uses: create multiple Site to Site VPN connections to increase bandwidth of connections to AWS
![[Pasted image 20230827165740.png]]
## Share Direct Connect between multiple accounts
![[Pasted image 20230827165835.png]]
