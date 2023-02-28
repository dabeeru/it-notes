

- **5 times faster than MySQL and 3 times faster than PostgreSQL at lower price**
- **Start at 10 Gb and scales with additional 10 Gb up to 64TB**
- Compute resources scalable to 32 vCPU and 244 GB RAM
- **2 copies of data are in each availability zone in minimum 3 zones = 6 copies of data**

### **High Availability**

Replicas types

- Aurora \(15\)
- MySQL \(5\)
- PostgreSQL \(1\)

Replicas additional information

- **MySQL replicas impacts aurora performance**
- MySQL replica can be in other region while aurora only in the same region as primary instance
- **Aurora replica support automatic failover**
- **MySQL replica can support different schema or data**
- Every replica has its own DNS address

### Backups

- Enabled by default
- **Do not impact performance**
- **Snapshot can be taken \(also do not impact performance\)**
- Snapshots can be taken from another AWS account

### Aurora Serverless

- **There is also Serverless version, which is scaling automatically based on needs**
- Billable on per invocation basis
