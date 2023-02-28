

### General informations

- Object storage \(files\)
- **Max size** of file **5TB**
- Unlimited storage
- Updates to objects make some time to propagate
- Files are stored in **buckets** which has **got globally unique names**
- Every **bucket gets its own DNS address**
- After uploading file to the **bucket** you will get **200 HTTP response**
- **Resource owner  AWS account** that **creates** Amazon S3 **bucket** and **objects**
- Objects stored in s3 consist:
    - **Key** \- filename
    - **Value** \- data
    - **Version**
    - **Metadata**
    - **Subresources**
        - **Access control list**
        - Torrents
- **Data consistency**
    - **Read after write**
    - Eventual consistency for overwrite PUT and DELETE \(there is time needed for propagation\)
- 99.99999999999% durability
- 99.99% availability
- Compression
    - GZIP
    - ZIP2

### **Tiers**  storage classes

- **S3 standard**  high availability and durability, can survive losing of 2 databases
- **Reduced redundancy \-** designed for noncritical, reproducible data that can be stored with less redundancy than the S3 Standard storage class, frequent access
- **IA** \- for non\-frequent rapid access, lower storage fee but additional fee for data retrieval
- **One zone  IA**  IA but in only one AZ, low cost but higher risk of data loss
- **Intelligent tiering**  Choosing tier based on machine learning algorithm which is analyzing how often specific data are retrieved
- **Glacier**  for archiving data, super cheap, but long retrieval time \(from minutes to hours\), retrieval fee
- **Glacier Deep Archive**  Lowest cost, retrieval time configurable in multiple hours, retrieval fee
- **Glacier Archive retrieval options**
    - **Expedited**  for files up to 250 mb, available within **5 minutes**
    - **Standard**  available within 3\-5 hours
    - **Bulk**  petabytes 5\-12 hours

### **Lifecycle management** \(moving between tiers for specific file age\)

    - Can be applied differently **for all current and future versions** of the files
    - Can be used to delete incomplete multipart uploads
    - Experation can be set up for object after specifgic period of time \(such objects are deleted\)

### **Versioning**

    - Can be only suspended after being enabled once
    - **For every version permission are resolved separately**
    - Integrated with lifecycle rules
    - Prevents for accidental deletions

### **Access control**

    - **Access control list**
    - **Bucket policies**
    - **Object policies**
    - **IAM policies** \(for users and groups\)

### Billing

- **Charging based on**
    - **Storage**
    - **Requests**
    - **Storage management pricing**
    - **Data transfer pricing**
    - **Transfer acceleration** \(if enabled\)
        - feature which uses CloudFront to speed up transfer to end users
        - Works for files up to 1Gb
    - **Cross region replication pricing** \(if enabled\)
- **Encryption**
    - **In transit  SSL/TLS** \- to secure transit
    - **On rest  encryption of storage data**
        - **S3 managed keys  SSE\-S3**
        - **AWS Key management service keys  SSE\-KMS**
        - **Customer Provided keys  SSE\-C**
        - **Client\-side encryption**  data can be encrypted on client side and uploaded

### Performance

- **Reading simultaneously for different prefixes** will increase your application performance
- Setting up date based prefixes can be used to boost performance
- Take into account that using **KMS can decrease performance**, and add additional quota to your calls
- **Multipart uploads**
- **Multipart downloads**

### Cross\-region replication

- You need to configure **replication role**
- You can **set up filters** to replicate **only specific files**
- Replication **can work for bucket** in and **outside your AWS account**
- It works **only for new versions** in the bucket
- **Versioning must be enabled**

### Transfer acceleration

- It allows you to **upload file to CloudFront edge location** rather than directly to S3 which is speeding it up
- **There is a tool for checking how much it will be speeded up** in practice

### S3 Event notifications

- we can trigger lambda function on S3 event, put something on DLQ if lambda processing fails
- Notifications can be sent to **SQS, SNS** and lambda functions
- Versioning should be enabled
- You can specify a filter on what data should trigger notification
- Events
    - Object created
    - Object Removed
    - Object Restored \(while restoring from glacier\)
    - RRS Object Lost\- when reduced redundancy object is lost
    - Replication  when replication failed or exceeds 15m
