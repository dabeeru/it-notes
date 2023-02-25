---
title: Kafka
creation_date: 2023-02-01
tags:
	- ToBeRefined
	- IT
	- DataProcessing
	- DataPipeline
	- CatalyX
---
# Event streaming
Event streaming is about capturing data from multiple sources in real-time, so it can be encapsulated in the data stream which can be analyzed in the real-time or retrospectively by multiple consumers.

Data sources can be for example, databases, sensors, mobile devices, cloud services etc.

Streams can be persisted for later retrieval, maniupulated and processed.

# Use cases 
- Processing financial transaction in real time
- Monitoring fleet
- IoT devices
- Reacting to customer orders
- Being a foundation for data platforms, event-driven architectures, microservices

# Architecture
- Brokers - responsible for storing the data
- Kafka connect - continuously importing and exporting data as event stream
- Client libraries - used to integrate various applications with Kafka cluster

# Concepts
## Event
Event is bject representing the fact that omething happened, everything which can be write or read from Kafka has to be an event (or collection of events).

Event consist:
* key
* value
* timestamp

## Producer & Consumers
- Producers are saving data into Kafka while Consumers are reading it
- Both are fully agnostic and decoupled from each other

## Topics
- Events are orginzed and stored as topics. 
- Topics is like a folder in filesystem, and events are like files
- Every topic can have 0, 1 or multiple consumers and producers
- Events are not deleted from the topic after being processed 
- It's matter of configuration on how long the data will be kept in the topic
- Topics are partitioned - spread acros number of *buckets* located on different Kafka brokers
- Partitioning enables scalability - data can be read from multiple nodes at ones
- Events with the same id are saved in the same partition and Kafka guarnatees that they will be read in the same order as they were written

## Controller (Leader)
Kafka controller is an elected broker which is responsible for:
- managing paritions
- partition log pair leadership (?)
- managing replicas

### Controller election
- Controller election is done by [[Zookeeper]]
- Every node when registered is connecting to zookeeper and creating related *znode* 
- First node is getting znode cluster
- When controller is elected then the *controller epoch* is established and broadcasted across the nodes - this ensures that all messages from prvious contoller (so previous epoch) will be ignored
- If controller fails then process begins again

# Configuration
- Kafka uses [[Zookeeper]] for:
	- configuration managment
		- topic configuration
		- broker list
		- ACLs
		- quotas 
			- network bandwidth
	- service discovery
	- leader node election

### Zookeeper replacement
Kafka optionally Kafka can use [[Raft]] (KRaft) algorithm as consensus negotiation algorithm which replace zookeeper, but this feature is not considered production ready yet. 

KRaft is Sefl-Managed Metadata Quorum mode, where instead of Zookeper Kafka broakers uses [[Raft]] algorithm to achieve consensus.

- KRaft ensures that the leader is elected
- *Leader* is responsible for log replication to the followers
- *Leader* utilizes regular heartbeats to inform *followers* of its status
- If *follower* doesn't get the heartbeat it changes it's status to *candidate*

# High availability
- Topics can be replicated for high availability 
- Common replication factor is 3 
- Replication is made on level of topic-partitions

# Performance
- Kafka's performance is not impacted by size of the data so data can be stored for long periods

# API
- Admin API - administrtion 
- Producer API - writing steam of events to Kafka topics
- Consumer API - subscribing to the topics
- Kafka Stream API 
	- for implementing stream processing applications
	- exposes functionalities of transformations, aggregtions, joins, windowing etc.
	- allows to transform data from ane topics into the another topic
- Kafka Connect API
	- API which allows to build various connectors (there are a lot of community connectors)

# Use cases
- Messaging 
- Website Activity Tracking
- Metrics
- Log Aggregation
- Stream processing - data can be pushed through multistage pipeline build on Kafka where in each stage we are appling some transformations
- [[Event sourcing ]]

# Other
- Kafka can guarnatee that single event will be processed exactly once