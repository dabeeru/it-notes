

- **Lets you connect your data center with AWS storage solutions**
- Its syncing data from your data center with the cloud
- It can be **VM image** compatible with **MS Hyper\-V and VMWare ESXi**
- You can define volumes used for syncing on the VM

### Types

- **Volume gateway ISCSI** \(syncing with EBS\)
    - **Stored volumes**  you always have full copy of the data
    - **Cached volumes**  data only kept on the volume temporarily before upload to the cloud or caching data during download
- **File gateway \- NFS&SMB** \(syncing with s3\)
- **VTL  Tape Gateway**  let's you leverage your tape backup infrastructure to store backup data to the cloud \(instead of real tapes\)
- \(not working with EFS\)

---
