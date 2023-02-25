---
title: AWS Billing
creation_date: 2023-02-11
tags:
	- IT
	- Cloud
	- AWS
	- Billing
---
- You can set up billing **alarm in [[AWS cloudwatch]]**
- You can use [[AWS Cost Explorer]] \(doesn't support attributing costs to multiple tenants\) and [[AWS Cost and Usage Reports]] to generate some reports and export it to [[AWS S3]] bucket
- You can use [[AWS Athena]] and [[AWS QuickSight]] to aggregate the reports and visualize it

## Cost allocation
- Options for cost allocations are not ideal, if we are hosting multi\-tenancy application it might not be easy to allocate cost of shared databases traffic etc.
- In general, first step is to measure tenant activity, and come up with the price for specific actions, which is calculated from cost that we can allocate and proportional part of shared costs 
- There is [[AWS Application Cost Profiler]] which allows more capabilities for attibutin cost to specific application

### Cost allocation tags
- You can configure special tags which influence the Billing report and allows you to allocate costs to specific consumers
- allocation tags types
    - AWS generated tags 
    - user\-defined tags
- AWS generated tags requires activation

## Saving plan
   - you can **define** amount of usage in **$/hour** in **1 or 3 years** term
   - You are **billed lower rate up to this commitment**
   - Works for **Lambda, EC2, Fargate**