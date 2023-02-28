---
title: Raft
creation_date: 2023-02-01
tags:
	- IT
	- High availability
	- Kafka
---
Raft is a consensus algoritm used in higly available archotectures.

[This site](https://raft.github.io/) shows the election mechanism really nicely.

## Leader election
term - a period withing a specific node in the cluster is a leader
1. If current Leader fails or if the algorithm is initialized then new term begins and election is started
2. Election is started by candidate node - the node becomes candidate if it won't get heartbeat frot the leader in the randomized timeout period
3. Term counter is increased, candidate votes for itself and then ask other nodes for votes
4. If the node will get majority of votes then it's being a new leader
5. If there will be a new cadidate broadcasting higher term number it will get precdence in voting
6. If there will be a split voting the new term is started

## General characteristics
- Leader node is responsible for log entries replication
- Every node in the cluster has it's own state machine and log of operation
- From the outside, the cluster behaves like one state machine
- Subject of consensus are the proper values for the proper values of the operation log
	- So if one node set value *x* as *n*th operation of the log no other node can perform another *n*th operation
- As long as majority of nodes is available state of the cluster will be saved
- Algorithm also guarnatees that no invalid information can be returned

https://raft.github.io/
https://en.wikipedia.org/wiki/Raft_(algorithm)
https://developer.confluent.io/learn/kraft/