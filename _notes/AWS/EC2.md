---
title: EC2
creation_date: 2023-02-13
tags:
	- IT
	- Cloud
	- AWS
	- Compute
	- EC2
---
# EC2

## Hypervisors
- **XEN**
- **Nitro** \(Developed by AWS\)

## Windows instances
- Windows instances are **billed by full hours**
- Storage is billed even if instance is idle

## Pricing models
### On\-demand
- no commitment, hourly rate

### Reserved 
- 1 or 3 years Terms, significant discount
- Types
	- **Standard**  up to 75% off, you have to pick type of instance for whole contract
	- **Convertible**  up to 53% off, you are reserving resources but you can use different types of EC2 machines
	- **Scheduled**  you are getting your machines only in specified time window
- RI Utilization - show how much reserved instances are utilized
- RI Coverage - shows how much of all ec2 is handled by reserved instances
	
### Spot
- you decide the price that you want to pay, 
- if current price \(which is calculated based on available cloud compute resources\) is lower the you are getting your machine
- You are not charge for partial hours where AWS terminated your instance
    - You are charged for partial hours where you terminated your instance
- Up to 90% compared to On\-Demand
- **Spot blocks**
    - You can set it for your spot instance in order to **stop it from being terminated** if price is to high
    - Spot block can last from one to six hours
- Spot ec2 instance request
    - **Maximum price**
    - **Desired number**
    - **EC2 spec**
    - **Request type**
        - **One\-time**
        - **Persistent**
    - **Valid from, valid until**

#### **Spot fleets**
- **Collection of spot and \(optionally on\-demand\) instances**
- You can set up different launch pools for different EC2 types, AZs, OS types
- **Strategies**
    - **CapacityOptimizes**  stop instance come from the pool with optimal capacity for the number of instances launching
    - **Diversified**  distributed across all polls
    - **LowestPrice** \(default\) \- stop instance come from the pool with the lowest price
    - **InstancePoolsToUseCount**  The spot Instances are distributed across the number of Spot Instance pools you specify. Used together with lowestPrice

### Dedicated host 
- you can use your own license and get physical server instance \(on\-demand or Reserved\)

## Tenancy
- You **cannot change** this attribute **between host/dedicated and default** after launching instance

### Default
- hosted on shared hardware

### Dedicated 
-  hosted on single\-tenant hardware

### Host 
- hosted on isolate server with configuration which you control

## Instances types
![[Untitled 3.png]]

## Network interfaces

### ENI  virtual network card
  - Gets at least one private IPv4 address
  - One or more IPv6 addresses
  - Can get one public IPv4 address
  - Gets MAC address
  - Create managed network \(?\)
  - Use network and security appliances in your VPC \(?\)
  - Create dual homed instances with workloads/roles on different subnets\(?\)
  - Create a low\-budget, high\-availability solution
	
###  EN  - Enhanced networking
- uses single root I/O virtualization for high performance
- Higher I/O performance
- Lower CPU utilization
- Higher bandwidth, lower latencies
- No additional charge
- Types
	- ENA \(preferred\) - Up to 100 GBps
	- Virtual function \(Intel 82599\) \- up to 10 gbps
	
###  Elastic Fabric Adapter \(EFA\)
- network device for High performance computing \(HPC\), can be attached to EC2, mostly for machine learning purposes
- lowest latency
- highest throughput
- Machine learning applications can connect to it **bypassing the OS kernel** \(only on linux\)
- You cannot move it to antoher AZ after creation

## Instance Metadata
- You can curl **[169.254.169.254/latest/user\-data](http://169.254.169.254/latest/user-data)**to get boot script for the instance
- **[169.254.169.254/latest/meta\-data/](http://169.254.169.254/latest/meta-data/)\<meta\-data\-name\>** \- to get multiple different metadata about the instance

## Autoscalling
- **Groups**  group of components being scaled \(eg. applications, DBs\)
- **Configuration templates**  configuration used to create EC2 instances \(AMI ID, instance type, key pair, security groups block device etc. \)
- **Scaling options**  rules on how and when autoscaling is performed
### Scaling options
- **Maintain instance levels**  if health check fail, instance is terminated and new one is created
- **Scale manually**
- Scale **based on schedule**
- Scale **based on demand**  scaling based on **CPU utilization**, **Networks bytes in/out**, **ALB request** per target
- Use **predictive scaling**  predicted automatically based on previous performance

## Placement groups
- **You can move existing instance into placement group**
- **One machine can be only in one placement group**
- **Placement group cannot span over multiple regions**
- Single EC2 instance can be hosted in only one placement group at the time
- **Spread placement group** can have **maximum 7 instance per AZ**

### Clustered placement groups
- **Grouping in single AZ**
- **Low latency, high throughput**
- **EC2 instances within clustered placement group should be homogenous**
				
### Spread placement group
- **Run on different hardware**
- **Can be in different AZ**
- **Individual critical EC2 instances**
				
### Partition placement group
- **Multiple servers in partitions**
- **Partitions needs to be in different racks**
- Multiple EC2 instances \(e.g. HDFS, HBase, Cassandra\)
- Only specific types of EC2 can be put into Placement group
	- Compute Optimized
	- GPU
	- Memory Optimized
        - Storage Optimized
        - **You cant merge placement groups**
				