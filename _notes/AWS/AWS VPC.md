# VPC
![[Untitled.png]]
## Subnet
- You can select IP ranges and create subnets 
- subnet belongs to only one AZ

### Public subnet
A subnet which has **at least one route to IGW**

## NACLs
- **Network Access Control List**
- **NACL** 1:* **Subnets**
- Rules with lower number has priority
	- Its recommended to set prio to rule by increment of 100
- Default NACL allows all outbound and inbound traffic
- Evaluated before **Security Groups**
- **Stateless** - any changes applied to incoming rule wont be automatically applied to outgoing rule
- NACL affect only traffic which is going in and out to the subnet, not a traffic withing
- Rules need to take into account ephermal ports
	- If host in VPC is a client then inbound rule for the epermal port range must be creted
	- if host in VPC is a server then outbound rule for ephermal port range must be created 

### Blacklisting IPs
- You can use **NACL** to block malicious IPs
- With ALB **NACL** needs to be set up in **ALB subnet**, and via security group we can configure that only traffic from ALB is possible \(with ALB original connection is terminated on ALB so target machine doesn't know origin IP\)
- With NLB you can simply use sybnet NACL 

## Security groups
- All outbound traffic is allowed by default \(?\)
- All inbound traffic is allowed by default \(?\)
- **All inbound traffic is blocked by default***
- **All outbound is allowed by default**

- **Stateful** - Any changes applied to incoming rule will be automatically applied to the outgoing rule 
- Changes in security groups takes effect immediately
- **You can have multiple security groups added to ec2 instance**
- You cannot block specific IP address using security groups
- **You can specify only allow groups not deny**

## Routing tables
- There is default route in routing table which allows you to communicate between AZ within VPC

## Internet gateway
 - One per VPC

## NAT
### NAT Gateway
- It is managed NAT server
- Redundant within AZ
- Scalable from 5 Gbps to 45 GBps
- If having resources in multiple AZ it is good idea to have one NAT Gateway in each of AZ
- Ephermal ports range 1024\-65535

### NAT instance
- NAT Instance is EC2 instance which is performing NAT Gateway role
- *You need to disable source/destination check*
- Its not scalable automatically
- NAT instance must be in public subnet
- HA and autoscaling must be implemented  

## ElasticIP
- Elastic IP  - public IP address which **can be assigned to EC2**

## Other
### VPC reserved addresses
- **[10.0.0.0](http://10.0.0.0) \- Network address**
- **[10.0.0.1](http://10.0.0.1) \- VPC Router**
- **[10.0.0.2](http://10.0.0.2) \- DNS server \(also present as second address in each subnet\)**
- **[10.0.0.3](http://10.0.0.3) \- Reserved by AWS for future use?**
- **[10.0.0.255](http://10.0.0.255) \- Network broadcast address \(broadcast is not working\)**
- You can have up to **5 VPCs on AWS account** by default quota

### Default VPC
- all instances in default VPC have internet access
- All instances in default VPC both private and public IPs
- When creating VPC following resources are created
    - **Security group**
    - **Network ACL**
    - **Route table**

## VPC peering
- Allows to connect to private VPCs via direct network route
- It can be done between different accounts or different regions
- VPC has to be peered directly it is not possible to root traffic true third VPC

## VPC Flow logs
- **Logs from IP traffic**
- Stored in Amazon CloudWatch Logs
- **Levels**
    - **VPC**
    - **Subnet**
    - **Network interface**
- You cannot create flow log with VPC from another account \(even if peered\)
- Exclusions
    - **Traffic to Amazon DNS server**
    - **Traffic generated by Windows instance for Amazon Windows license activation**
    - **Traffic to and form [169.254.169.254](http://169.254.169.254) for instance metadata**
    - **DHCP traffic**
    - **Traffic to reserved IP address for the default VPC router**

## Pricing
- **Inbound connection are free**
- **Connection** inside AWS \(using private IP\) **within single AZ are free**
- Traffic **between AZs,** **~$0.01 per GB**
- Traffic **through internet** \(with public addresses\) **~$0.02 withing single Region**
- Traffic between regions also $0.02

## Egress-only internet gateway
- **allow IPv6 based traffic** within a VPC to **access the internet**
- denying any internet-based resources to connection back into the VPC

## Bastion hosts
- Special purpose computer in DMZ, used to handle acccess from untrusted networks and computers \(all other functions should be limited\)
- HA with Bastion hosts
    - You can set up **network load balancer** before your bastion instances to have it in **active/active** mode
    - You can also set up in **active/passive**, by simply creating bastion host in **autoscaling group in \(1 min, 1 max instances\)**