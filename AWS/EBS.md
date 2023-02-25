# EBS

## Disk types
- **Cold HDD  250 IPOS / 250 MiB/s**
- **Throughput optimized HDD  500 IOPS / 500 MiB/s**
- **General purpose SSD \- 16 000 IOPS / 1000 Mib/s**
- **Provisioned IOPS SSD \- 256 00 IOPS / 4000 MiB/s**
- **Magnetic  40\-200 IOPS**

Root disk can be only:
- **General purpose SSD**
- **Provisioned IOPS SSD**
- **Magnetic**

## Snapshots
- **Snapshot of EBS volumes are incremental**
- **But every snapshot consist all the data needed to restore ebs \(?\)**
- **To make a snapshot of the root drive you should stop an instance**

## AMIs
- You can't delete a snapshot of the root device of an EBS volume used by a registered AMI. You must first deregister the AMI before you can delete the snapshot
- AMI virtualization types
    - PV
    - HVM \(much more ec2 instances support it\)
- AMIs types
    - EBS volume  created from EBS snapshot \(supported for most instances type\)
    - Instance store volume \(ephermal storage\)  created from the template stored on S3 \(supported by only couple instances types\)
        - All instance volumes has to be attached to EC2 during its creation
        - EC2 with instance store volumes cannot be stopped
        - In case of hypervisor failure all data will be lost

## EBS Hibernate
- **Allows you to perform EC2 hibernation  save the contents form RAM to EBS volume**
- Hibernated instance after being waked up retains its instance ID
- Boot is much faster, all processes are resumed
- Useful for workloads where processes cannot be interrupted or for services which are booting lot of time
- Uptime wont be restarted
- **RAM** must be **less than 150 GB**
- **No more than 60 days of hibernation**
- Available only for On\-Demand and reserved instances
- Not available for autoscalling group

## **Other**

- **All EBS volumes are replicated between availability zones automatically**
- EBS drives can be resized \(filesystem has to extended\)
- **One EBS drive can be replicated in another AZ by taking a snapshot and creating AMI from it, you can move it also to another region but you need copy AMI there firs**t
- Root and EBS drives can be encrypted
- **Default action for EBS root drive when instance is being terminated is deletion**
- **Additional EBS drives are not deleted by default during termination of ec2 instance**
- You have to turn on **termination protection** if you want to
    **To ensure that EBS drive is not deleted check DeleteOnTermination attribute**
- Snapshots of encrypted volumes are encrypted automatically
- Volumes restored from encrypted snapshots are encrypted automatically
- You can share only unencrypted snapshots
- You can encrypt root volume while creating new instance
- How to create encrypt unencrypted Root device
    - Provision EC2 instance
    - Create snapshot of root device
    - Copy and encrypt snapshot
    - Create AMI from snapshot
    - Use AMI to create new EC2 with encrypted root volume
    - Network interfaces
