---
aliases:
- ASG 
---
# Auto Scaling Group (ASG)
- goal is to automatically scale in/out to address the load
- set a min/max
- set scaling policies
- automatically register new instances to a load balancer
- re-create instances in case a previous one is terminated (unhealthy)
- ASG is free, you pay for the instances
![[Pasted image 20230818160911.png]]
## Using Load Balancer
![[Pasted image 20230818160931.png]]
## Attributes
### Launch Template
- [[Scalable network architectures|AMI]] + instance type
- [[Scalable network architectures|EC2]] user data
- [[Scalable storage solutions|EBS]] Volumes
- [[Secure workloads and applications|Security Groups]]
- SSH key pairs
- [[Secure access to AWS resources|IAM]] Roles for instances
- Network + Subnets Information
- [[Scalable and loosely coupled architectures|ELB]] Information
## CloudWatch Alarms
- alarms monitor a metric (average CPU, custom metric)
	- averages are computed for the overall ASG instances
![[Pasted image 20230818161454.png]]
## Dynamic Scaling Policies
### Target Tracking
- ex. average CPU usage to stay around 40%
### Simple/Step Scaling
- alarm triggered, scale in/out
### Scheduled
- anticipate scaling based on known usage patterns
- ex. increase min capacity to 10 at 5pm on Fridays
### Predictive Scaling
- continually forecast load and schedule scaling ahead
### Metrics to use
- **CPU utilization**: average CPU across instances
- **RequestCountPerTarget**: make sure number of requests per instance is stable
- **Average Network In/Out**: if app is network bound
- **Custom metric**: push using CloudWatch
![[Pasted image 20230818162106.png]]

## Scaling Cooldowns
- after scaling activity, you will be in cooldown where ASG will not launch or terminate additional instances (to allow for metrics to stabilize)
- use AMI to reduce configuration time in order to serve requests faster and reduce cooldown period
