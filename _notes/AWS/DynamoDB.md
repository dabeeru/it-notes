

### **Core concepts**

- There are **tables**, **items** and **attributes**
- **Attributes** can be **scalar** or **nested** \(up to **32 levels deep**\)
- Every item needs to have **primary key**
    - **Primary key** can be **single value** \- Partition Key \- then primary key must be unique
    - **Composite Primary Key** \- **Partition Key and sort key** then Partition key don't need to be unique
    - Primary key is always subject of hash function which determines where exactly data is stored
- Hash function is deciding where to put specific item
- If using simple Primary key data are not kept in sorted manner
- When using **Composite primary key** then data **with the same partition key are saved close to each other** 
- There are also **transactions** that can span across **multiple items in multiple tables \(**Working with **25 items** and **4MB** of data\)

### **Partitions**

- Data are splitted into partitions based on their partition key value
- Every **partition is stored on SSD** drive and **replicated across multiple availability zones**
- Single partition can store up to 10GB data
- Single partition can serve **3000 RCU** and **1000 WCU** **per second**
- If read or write capacity exceeds partition limit, new partition is being created
- Adaptive capacity feature mitigates hot partition issue

### **Scans**

- Scan goes through all of data in the table and applying optional filtering after read
- It's reading entire pages \(1MB\) of data and the filtering
- This behaviour can be parallel by **adding** **segments** \- then **multiple workers will go trough the table in parallel**
- Scan can exceed both partition or table provisioned throughput
    - This can be mitigated by reducing page size or isolating data into another table \- then we can apply proper indexing

### **Indexes**

- Beyond primary key secondary indexes can be used to query the data by some attributes
- **Global secondary index** \- different partition key and sort key
- **Local secondary index** \- same partition key but different sort key
- there can be up to 20 GSI and 5 LSI
- **GSI**
    - **data for GSI** are copied into **different partition**
    - You cannot use strong consistency with GSI
        - But can you still run the query strong consistent query towards base table
- Attributes projection \- you need to decide which attributes will be copied for the index \(keys of base table are always projected\)
    - All
    - Include \(specific\)
    - Keys only
- LSI
    - You cannot create LSI after database has been created \(\!\)

### **Queries**

- By **Primary Key** or Index
- When querying you can obtain only specific attributes with proper filter

### **Backup & Restore**

- **Backups**
    - **Full backups on any time**
    - **Zero performance or availability impact**
    - **Backup** in the **same region as source table**
- **Point in time recovery**
    - **Not enabled by default**
    - **Up to 35 days**
    - **Incremental backups**
    - **Latest restorable**  **five minutes in the past**

### **Streams**

- **Time ordered sequence** **of UPDATE, INSERT, DELETE** operations in DB
- Stream can trigger lambda function which will process the entry
- **Available** for **24 hours**
- **Stream record includes single change** to the data
- Use cases
    **Multiple stream records** are **grouped within the shard**
    - **Cross region replication** \(**global tables**\)
    - **Relations between tables**
    - **Notifications** and **messaging**
    - **Aggregation, reporting, archiving** etc.
    - **Triggering lambda** functions

### **Consistency modes**

- Eventual consistency within second or less
- Strong consistency is not possible when using GSI
- Strong consistency eats more throughput capacity

### **Capacity considerations**

- **Modes**
    - **On demand**
        - single digit millisecond latency
        - When peak is being hit table is being scaled up doubling its capacity
            - eg. if there is 50 000 strongly consistent reads per second database is being scaled to 100 000 reads
        - Request units
            - Read request unit
                - one strongly consistent read or two eventually consistent read request for item up to 4KB size \(for bigger size of item unit are scalled up accordingly\)
                - for transactional ready you need 2 request units for 4 kb size item
            - Write request unit
                - 1 write units for item up to 1kb size \(if item is bigger it is scalled acorrdingly\)
                - 2 write request units for transaction write of items up to 1kb size
    - **Provisioned**
        - You specify in advance number of available capacity reads per second
        - You can buy some reserved capacity in advance to get a discount
        - It can be applied separately on base table and GSI
        - You can still set up some autoscalling capailites
- Secondary indexes inherit mode from main table
- Data is being saved on different partitions which number is based on the required capacity and data volume
- Adaptive capacity
    - AWS is allocating even resources to each partition, but in case of unevenly distributed load it's scaling the resources for hot partition, and until total amount of capacity for a table is not exceeded everything should be ok
    - but documentation is not fully clear.. it also says that it may take 5\-30 minutes to scale up the overloaded partition
- Burst Capacity \- when part of capacity is not used at the time is reserved for 5 minute long future burst
- Throttling will occur if throughput for single partition will exceed 3000 RCUs or 1000 WCUs

### Performance

- Generally it provides **Single digit millisecond latency** at any size
- **DAX**  **DynamoDB accelerator**
    - **Boost latency** from **milliseconds to microseconds**
    - No **cache integration** on **client side** is needed
    - **Read and write support**
    - Can be used for HA

### **Global Tables**

- **Managed Multi master Multi region replication**
- **Based on** **DB streams**
- Used for **DR** and **HA** \(multi region\)
- **Replication between regions** under one second
