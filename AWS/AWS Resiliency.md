---
title: AWS Disaster Recovery
creation_date: 2023-02-14
tags:
	- AWS
	- Cloud
	- IT
	- Disaster Recovery
---

[AWS Docs](https://docs.aws.amazon.com/whitepapers/latest/disaster-recovery-workloads-on-aws/disaster-recovery-workloads-on-aws.html)

# AWS Resliency 
## Definitions
**Resiliency** - ability to automatically recover from failure
**RPO - Recovery Point Objective** - determining to what extent we can accept loosing data 
**RTO - Recovery Time Objective** - determining how 
**Disaster** - unplanned event which is disrupting production workload and effectively impacting whole data center where the application is hosted
**BCP - Busines Continuity Plan** 
**MTBF** - Mean time between failures
**MTTR** - Mean time Between failures
![[Pasted image 20230214085007.png]]

## Good practices
- Spread workloads across the Availability Zones
- Deployment to multiple locations must be automated
- Application health needs to be monitored
- Disaster Recovery procedure must be tested

## Strategies
- Backup & Restore
	- continously backing up the data and habinf IaaC which can deploy the workload to another region
	- backup from all services can be saved on S3 and autoatically replicated into another region
	- [[AWS Backup]] can be very useful to create the backups automatically, it supports copying backups acros the regions
- Pilot Light
- Warm Standby
- Multi-site active/active