---
title: RDS
creation_date: 2023-02-01
tags:
	- IT
	- AWS
	- Cloud
	- Data
	- Database
	- CatalyX
---

Relational databases available on AWS, available engines:

- **MS SQL**
- **Oracle**
- **MySQL**
- **PostgreSQL**
- **Aurora**
- **MariaDB**

### **Multi\-AZ Disaster recovery**

- **Second RDS replicas** are deployed **in different AZ**
    **Active / Active**, switch by DNS
- When Primary AZ will go down then DNS is automatically switched to second AZ
- AWS is handling **replication automatically \(snchronously\)**
- Available for all database engines besides **Aurora** which is **Fault tolerant by design**
- You can **force failover by rebooting primary instance**
- **When enabled, backups are bade from secondary db so I/o for primamry is not interupted**

### **Read Replicas**  **performance boost**

- **Different URL** for **read replicas cluster**
- You can architect your system to write to write replica and read for read replicas
- Works for all db engines **besides MS SQL**
    New **data are replicated asynchronously** to all read replicas
- **Automated backup has to be enabled**
- You can have **up to 5 read replicas**
- You can have **read replicas of read replicas** \(?\!\)
- Each read replicas has it own DNS endpoint
- **Read replicas** can be in **multiple AZs** as well as **different regions**
- **Read replicas** can be used for **MultiAZ databases**
- Every **read replica can be promoted to autonomous Database** \(It breaks the replication\)
- You can create **Aurora Read replicas for other Database engines** \(can be also used to migrate your db to Aurora\)
- There might be some **propagation lag**

### Backups

- **Automated backups**
    - You can set **retention period up to 35 days**
    - **RDS** is doing **daily backups \(full snapshot of database storage volume\) plus transaction logs in eery 5 minutes**
    - When restoring RDS is choosing latest daily backup and applying transactions from transaction log on top of it
    - This let you to **restore** the database with **precision to seconds**
    - **Enabled by default**
    - **Backups stored in S3** \(storage is free of size of your database\)
    - **During backup window storage I/O might be suspended**
    - **Automated backups are deleted during deletion of RDS instance**
- **Snapshots**
    - **Full backup of the database**
    - It's **not deleted** when **deleting RDS instance**
- **Restore**
    - **Restore** is always made on **new RDS instance** with new DNS endpoint

### HA

- RDS Standby \(Secondary\) instances are only for failover, for increasing read performance you need to use read replicas

### **Encryption**

- Done by **KMS**
- **Encrypting storage data, automated backups** and **snapshots**
- If you want to encrypt unencrypted database you need to copy its snapshot encrypt it and create DB from it

### **Other**

- RDS is run on **VMs**, but **you cannot ssh to them**
- RDS is **not serverless** \(**Aurora Serverless is an exception**\)
- Aurora can invoke lambda functions
- You can shard your data into different DBS for write performance
- YOu can configure RDS database by creating parameters group and attaching it to DB instance
