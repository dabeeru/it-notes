

- Based on
    - Spark
    - Hive
    - Hbase
    - Flink
    - Hudi
    - Presto
- Cheaper
- Faster than standard Spark

### Architecture

- Cluster of EC2
- **Nodes types**
    - **Master  check health of the cluster and status of the task**
    - **Task \(optional\)  only run task not storing data**
    - **Core  run tasks and stores data in HDFS \(Hadoop file system\), at least one**
- Every node communicates with each other
- Logs from master log can be uploaded to S3 in 5 minute intervals \(needs to be configured on cluster creation\)
