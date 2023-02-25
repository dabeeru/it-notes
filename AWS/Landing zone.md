---
title: Landing zone
creation_date: 2023-02-06
tags:
	- IT
	- Cloud
	- CatalyX
---
---
- Blast radius concept - if some basic thing on AWS account will fail eveything on the account is affected
- Account baseline 
	- config rules, network policies, IAM etc.
	- can be divided into template which is most basic and shared between different account, and organization or even account specific 
- Scope of the landing zone
	- IAM managment
	- Global log aggregation
	- Enforcing VPC rules
	- Global audit

# Cloud specifc solutions
- AWS - [[AWS Landing Zone]]
- Azure - Cloud Adoption Framework (there are also official Terraform for it)
- GCP - ???

## Landing zones components
- Audit account 
- Multiple workload accounts
	- Different Organizations for different departments
	- Different accounts for different environments (at least PROD and non-PROD needs to be divided)
- Root accounts?
- Log audit accounts

## Some lings
[https://aws.amazon.com/solutions/implementations/aws\-landing\-zone/](https://aws.amazon.com/solutions/implementations/aws-landing-zone/)
[https://docs.aws.amazon.com/prescriptive\-guidance/latest/migration\-aws\-environment/understanding\-landing\-zones.html](https://docs.aws.amazon.com/prescriptive-guidance/latest/migration-aws-environment/understanding-landing-zones.html)
[https://docs.aws.amazon.com/prescriptive\-guidance/latest/migration\-aws\-environment/building\-landing\-zones.html](https://docs.aws.amazon.com/prescriptive-guidance/latest/migration-aws-environment/building-landing-zones.html)
[https://successive.cloud/cloud\-landing\-zone\-why\-organization\-need\-it/](https://successive.cloud/cloud-landing-zone-why-organization-need-it/)
[https://www.meshcloud.io/2020/06/08/cloud\-landing\-zone\-lifecycle\-explained/](https://www.meshcloud.io/2020/06/08/cloud-landing-zone-lifecycle-explained/)
