---
title: MSK
creation_date: 2023-02-01
tags:
	- ToBeRefined
	- Kafka
	- IT
	- Cloud
	- AWS
---
## MSK
- Managed streaming for Kafka
- support open source tooling

## Architecture
![[Pasted image 20230226165104.png]]
- Broker nodes
- zookeeper nodes

### HA
- Both Brokers and zookeper nodes can be deployed in multiple AZ
#### Best practices
- Set up three AZ cluster
- Replication factor must be at least 3
- in sync replicas set to RF - 1 
- Ensure that clients are aware of nodes in every AZ 

## Performance
- CPU load should be belo 60%

### Fail tolerance
- Fail tolerance - MSK automatically recovers from failure and reuses IP and storage from the older node  

### Storage
- After retention period data can be moved to low-cost storage
- You can scale to virtually unlimited storage

## MSK Serverless
- You can configure in serverless mode - all of the infrastructure will provisioned outside to the AWS Account
- It scales automatically
- PrivateLink can be used to manage connection to the cluster

## MSK Connect
- Allows to pull data from multiple datasources
- Scalles automatically) with multiple different tasks assigned![[Pasted image 20230226165029.png]]
- Connector can be extneded with multiple plugins which allows you to extract data from different datasources

## Security
- [[KMS]] encryption at rest
- TLS encyption in transit
- Authorization 
- You can use mutual TLS for connection between clients and cluster nodes
- You can set arbitrary credentials and store them in [[Secret Manager]] 