---
title: Quantum Ledger Database (QLDB)
creation_date: 2023-02-01
tags:
	- IT
	- Cloud
	- AWS
	- Data
	- Database
	- QuantumLedgerDatabase
	- CatalyX
---
# Quantum Ledger Database
Managed ledger database that provides transparent, immutable and cypthographically verifiable transaction log.

## Immutability

QLDB maintains immutable journal that stores a seqence of every data change. Journal cannot be modified, only new transactions can be added on top. Even if data is being deleted, it's just another entry on top, and data can still be accessed by reading the history.

## Integrity

Every transaction has it's cypthographic hash (secure summary also known a digest) which can be used to verify the integrity of the data.

## Scalability
- QLDB is serverless
- Can handle 2-3x more transactions than standard blockchain frameworks
- It's not decentralized like classick blockchains

## Other
- PartiQL support
- Nested document-oriented data model support
- [[ACID]] support
- Streaming capabilities - near real-time streaming through Kinesis Data treams also complete and verifiable, streaming can be used for
	- feeding [[Event Driven Architecture]] Applications
	- Analytics 
	- Replication to other data stores like [[Amazon Elasticsearch]]or [[RDS]]