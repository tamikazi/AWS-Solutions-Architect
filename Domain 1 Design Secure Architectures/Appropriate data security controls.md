---
aliases:
- KMS
- ACM
---
# Key Management Service (KMS)
- management of encryption keys
- fully integrated with IAM for authorization
- able to audit using [[Scalable and loosely coupled architectures|CloudTrail]]
## Key Types
- Owned Keys: free
- Managed Keys: free
- customer managed keys created in KMS: $1/month
- customer managed keys imported (must be symmetric): $1/month + pay for API calls
### Symmetric (AES-256 keys)
- single key to encrypt and decrypt
- AWS services that are integrated with KMS use symmetric
- never get access to the key unencrypted
### Asymmetric (RSA and ECC key pairs)
- public encrypt and private decrypt pair
- public key is downloadable but can't access private key unencrypted
- uses: encryption outside of AWS by users who can't call the KMS API
### Automatic Key Rotation
- Managed: automatic every year
- Customer Managed (must be enabled): automatic every year
- Imported: only manual rotation possible using alias
![[Pasted image 20230827142000.png]]
## Key Policies
### Default
- created if you don't provide a specific policy
- complete access to the root user = entire AWS account
### Custom
- define users. roles that can access key
- define who can administer the key
- useful for cross account access for the key
### Copying Snapshot across accounts
1. create snapshot encrypted with your own KMS key (customer managed)
2. attached key policy to authorize cross-account access
3. share encrypted snapshot
4. create copy of the snapshot, encrypt with a CMK in your account
5. create a volume from the snapshot
## Multi Region Keys
![[Pasted image 20230827142714.png]]
- same keys in different regions that can be used interchangeably
- encrypt in one region and decrypt in other regions
- they are **not** global
- each key is managed independently
![[Pasted image 20230827143106.png]]
## S3 Replication Encryption Considerations
- unencrypted and encrypted with SSE-S3 objects are replicated by default
- objects encrypted with SSE-C are never replicated
- can use multi region KMS key but they are currently treated as independent keys by [[Scalable storage solutions|S3]] (object will still be decrypted and then encrypted)
### For KMS
- specify which key to encrypt the objects within the target bucket
- adapt key policy for target key
- IAM Role with KMS: decrypt for the source key and KMS: encrypt for the target KMS key
## AMI Sharing Process Encrypted via KMS
1. AMI in source account is encrypted with KMS Key from source account
2. must modify image attribute to add a launch permission which corresponds to the specified target account
3. must share the keys uses to encrypt the snapshot the snapshot the AMI references with the target account / [[Secure access to AWS resources|IAM]] Role
4. IAM Role/User in the target account must have the permissions to DescribeKey, ReEncrypted, CreateGrant, Decrypt
5. when launching instance from AMI, optionally the target account can specify a new key in its own account to re-encrypt the volumes
![[Pasted image 20230827144121.png]]
# Certificate Manager (ACM)
- provision, manage and deploy [[Technical Terms|TLS]] certificates
- provide in flight encryption for websites
- supports public and private [[Technical Terms|TLS]] certificated
- automatic certificate renewal
- integrations with [[Scalable and loosely coupled architectures|ELB]], [[Scalable network architectures|CloudFront]] distributions, [[Scalable and loosely coupled architectures|API Gateway]]
- cannot use with [[Scalable network architectures|EC2]]
![[Pasted image 20230827145322.png]]
## Requesting Public Certificates
1. list domain names to be included int he certificate
2. select validation method ([[Technical Terms|DNS]] or email)
3. few hours to get verified
4. certificate will be enrolled for automatic renewal
## Importing Public Certificates
- option to generate outside of ACM then import
- no auto renewal
- sends daily expiration events starting 45 days prior to expiration
- [[Scalable and loosely coupled architectures|Config]] has managed rule to check for expiring certificates
![[Pasted image 20230827145816.png]]
## Integration with ALB
![[Pasted image 20230827145855.png]]
## Integration with API Gateway
- create custom domain name in [[Scalable and loosely coupled architectures|API Gateway]]
### Edge Optimized (default)
- for global clients
- routed through [[Scalable network architectures|CloudFront]] edge locations
- [[Scalable and loosely coupled architectures|API Gateway]] still lives in only 1 region
- [[Technical Terms|TLS]] certificate must be in the same region as [[Scalable network architectures|CloudFront]] in us-east-1
- set up CNAME or A-alias record in [[Highly available and fault tolerant architectures|Route 53]]
### Regional
- for clients in the same region
- [[Technical Terms|TLS]] certificate must be imported on [[Scalable and loosely coupled architectures|API Gateway]] in the same region as the API stage
- set up CNAME or A-alias record in [[Highly available and fault tolerant architectures|Route 53]]
![[Pasted image 20230827150315.png]]
