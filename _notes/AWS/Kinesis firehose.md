

**S**treaming service in AWS

### **Streams**

- can receive data from different sources
- Data areas stored in shards \(every shard can have different purpose\)
    Data are stored for 24g  7d retention
    - Shard has 5 transaction per second for reads
    - 2MB per second
    - Up to 1000 records per seconds for writes
    - Total data write rate of 1MB per second
    - Capacity of the stream = sum of capacity of all shards
- Data can be streamed to some consumers

### **Firehose**

- Elements
    - **Delivery streams**
    - **Records of data**
    - **destinations**
- Can receive data from different sources
- **No storage**
- Data has to be **processed straight away** with eg. **Lambda** function and stored in **S3** or **Elasticsearch cluster**
- Its easiest way to load streaming data into data stores and analytical tools

### **Analysis**

- You can use it to analyze your data inside Kinesis Streams or Kinesis Firehose

### Other

- Autoscalling can be used to scale Kinesis Strems  clowatch can trigger api gateway to call lambda which can change Kinesis Data Stream Shards nuber
- YOu can also use Kinesis Scaling Utilities  automated withj AWS Beanstalk
