---
tags:
- EC2
- Cost
aliases:
- Purchasing Options
- On-Demand
- Reserved
- Spot
---
# EC2 Instance Purchasing Options
## On-Demand
- short workload, predictable pricing
- Linux/Windows - per second, all others per hour
- highest cost but no upfront payment
- recommended for **short-term** and **un-interrupted workloads** where you cannot predict how the application will behave
- ***coming and staying at a resort whenever***
## Reserved (1 & 3 years)
### Reserved Instances
- long workloads
- 72% discount from On-Demand
- more discounts for longer periods
- more discounts if paid more upfront
- scope: Regional or Zonal
- can buy and sell in the Reserved Instance Marketplace
- recommended for **steady state usage apps (databases)**
### Convertible Reserved Instances
- long workloads with flexible instances
- 66% discount
- able to change instance type

- ***planning ahead, if you stay a long time, you'll get a discount***
## Savings Plans (1 & 3 years)
- commitment to an amount of usage, long workload, similar to Reserved
- up to 72% discount
- commit to a certain type of usage (ex. $10/hour for 1 or 3 years)
- usage beyond in billed at the On-Demand price
- locked to a specific instance family (ex. t2) and AWS region (ex. us-east-1)
- ***pay a certain amount per hour for a certain period in any room type***
## Spot Instances
- short workloads, cheap, can lose instances (less reliable)
- discount up to 90%
- Useful for:
	- **Batch jobs**
	- **Data analysis**
	- **Image Processing**
	- Any **distributed workloads**
	- Workloads with **flexible start/end times**
- **Not suitable for critical jobs or databases**
- ***people bid on empty rooms and the highest bidder gets the room, can get kicked out at any time***

### Spot Instance Requests
- define **max spot price**
	- you have the instance as long as **current price** < **max price**
#### Spot Block
- ***block*** spot instances during a specified time (1 - 6 hours) without interruptions
### Termination
![[Pasted image 20230817215439.png]]

### Spot Fleets
- set of spot instances + (optional) on-demand instances
- will try to meet target capacity with price constraints
- automatically request spot instances with the lowest price
- Strategies:
	- **lowestPrice**: from the pool with the lowest price (cost optimization, short workload)
	- **diversified**: distributed across all pools (great for availability, long workloads)
	- **capacityOptimized**: pool with the optimal capacity for the number of instances
	- **priceCapacityOptimized** (recommended): pools with highest capacity available, then select the pool with the lowest price (best choice for most workloads)
## Dedicated Hosts
- book an entire physical server, control instance placement
- allows you to address **compliance requirements** and use **existing server-bound software licenses**
- can do on-demand or reserved purchasing
- most expensive option
- ***booking an entire building of the resort***
## Dedicated Instances
- no other customers share your hardware
- may share hardware with other instances in the same account
- no control over instance placement (can move after stop/start)
## Capacity Reservations
- reserve capacity in a specific [[Secure access to AWS resources|AZ]] for any duration
- can always have access to EC2 capacity when needed
- no time commitment
- no billing discounts
- combine with Reserved and Savings plans to benefit from billing discounts
- charged at on-demand rate regardless of usage
- recommended for **short-term, uninterrupted workloads that need to be in a specific AZ**
- ***book a room for a period even if you don't stay in it***

# RDS Read Replicas - Network Cost
- no cost for replicas within the same region
![[Pasted image 20230819151530.png]]
