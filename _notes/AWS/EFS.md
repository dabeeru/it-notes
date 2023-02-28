

- EFS volumes can be **shared between EC2s**
- **Filesystem grows automatically**
- There is **bursting / provisioned throughput** options
    **EFS Lifecycle ** if data is **not used fox x days** it can be moved to cheaper **Infrequent use storage class**
- Performance modes  **General Purpose** and **Max I/O**
- Encryption can be enabled with KMS
- Communication is made over **NFS** \(v4\)  Security groups must reflect that
- You can enable TLS transport encryption during the mount
- You pay only for storage you use
- It scales to petabytes
- Data is stored in multiple AZ
- Read after write consistency
- EFS mount helper  tool for mounting EFS volumes can also enable transit encryption by creating tunel
