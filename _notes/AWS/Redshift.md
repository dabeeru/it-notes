

AWS data warehouse \(solution for OLAP\)

- **Enhanced routing**  VPC resources get access to redshift
- Data warehousing
- Business analytics
- Petabyte scale

### Modes

- **Single node \(160 Gb\)**
- **Multinode**
    - **Leader node  manages client collections and receives queries\)**
    - **Compute node stores data and performs queries \(max to 128 nodes\)**

### **Advanced compression**

- **compressing columns** rather than rows
- **Sampling data in column** and chooses **best compression algo**

### **Massive Parallel Processing \(MPP\)**

- **Automatically distributes data and query loads between nodes**

### Backups

- 1 day retention period by default \(up to 35 days\)
- **Three copies  original, replica** on **compute node** and **S3**
- **Redshift can asynchronously replicate snapshots to s3 in another region for disaster recovery**

### **Pricing**

- **Compute node hours**
    - Not charged for leader node
- **Additional pricing for backup and data transfer**

### Security

- **Encrypted in SSL**
- **AES256 encryption on rest**
- Keys handled internally by redshift by default
- **KMS or HSM \(own keys\) can be used instead**

### **Availability**

- **Only 1 AZ**
- **Can restore snapshots in different AZ**
