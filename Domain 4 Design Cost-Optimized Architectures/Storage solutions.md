---
aliases:
- FSx
- 
---
# FSx
- launch 3rd party high performance files systems on AWS
- fully managed
## Windows
- supports SMB protocol and NTFS
- Microsoft Active Directory integration
- can be mounted on Linux [[Scalable network architectures|EC2]] instances
- supports Microsoft's Distributed File System (DFS) Namespaces
- 10's GB/s, millions of IOPS
- supports HDD and SSD
- can be accessed from on premises
- can be configured to multi-AZ
- data backed up daily to [[Scalable storage solutions|S3]]
## Lustre
- parallel distributed file system for large-scale computing (machine learning, **high performance computing (HPC)**)
- video processing, financial modelling, electronic design automation
- scales to 100's GB/s, millions of IOPS
- supports HDD and SSD
- can read [[Scalable storage solutions|S3]] as a file system and write the output of the computations back
- can be used from on premise servers
### Scratch File System
- temporary storage
- data not replicated (doesn't persist if server fails)
- high burst
- useful for short term processing and optimizing costs
### Persistent File System
- long term storage
- data replicated within same AZ
- replace failed files within minutes
- useful for long-term processing, sensitive data
## NetApp ONTAP
- compatible with NFS, SMB, iSCSI protocol
- move workloads running on ONTAP or NAS to AWS
- works with all OS
- storage shrinks/grows automatically
- snapshots, replication, low cost, compression and data de-duplication
- point in time instant cloning, useful for testing new workloads
## OpenZFS
- compatible with NFS
- move workloads running on ZFS to AWS
- works with all OS
- up to 1,000,000 IOPS
- point in time instant cloning, useful for testing new workloads