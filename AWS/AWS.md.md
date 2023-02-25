# AWS

# Abstract

## Landing zone

[https://aws.amazon.com/solutions/implementations/aws\-landing\-zone/](https://aws.amazon.com/solutions/implementations/aws-landing-zone/)

[https://docs.aws.amazon.com/prescriptive\-guidance/latest/migration\-aws\-environment/understanding\-landing\-zones.html](https://docs.aws.amazon.com/prescriptive-guidance/latest/migration-aws-environment/understanding-landing-zones.html)

[https://docs.aws.amazon.com/prescriptive\-guidance/latest/migration\-aws\-environment/building\-landing\-zones.html](https://docs.aws.amazon.com/prescriptive-guidance/latest/migration-aws-environment/building-landing-zones.html)

[https://successive.cloud/cloud\-landing\-zone\-why\-organization\-need\-it/](https://successive.cloud/cloud-landing-zone-why-organization-need-it/)

[https://www.meshcloud.io/2020/06/08/cloud\-landing\-zone\-lifecycle\-explained/](https://www.meshcloud.io/2020/06/08/cloud-landing-zone-lifecycle-explained/)

## North\-south, east\-right model

It's a model describing different types of traffic in data center/cloud environment

- North traffic \- traffic going outside 
- South traffic \- traffic going inside
- East\-West \- internal traffic between hosts in private network

In general East\-West traffic was considered as trustworthy while North and South as untrustworthy, but modern zero trust approach to security assumes that every traffic should be untrusted by default. 

...

[https://aws.amazon.com/blogs/networking\-and\-content\-delivery/deployment\-models\-for\-aws\-network\-firewall/](https://aws.amazon.com/blogs/networking-and-content-delivery/deployment-models-for-aws-network-firewall/)

---

# General

- There are **Regions** and **availability zones \(AZ\)**
- Availability zone is single and group of data centers 
- **Region** has **2\-3 availability zones** 
- **Edge location** – location where data is cached, used in **CloudFront** 
- **Number of Edge Locations \> Number of Availability Zones \> Number of Regions** 
- **Support types** 
    - Basic
    - Developer
    - Business
    - Enterprise

## AWS Control tower

...

## Billing

- You can set up billing **alarm in cloudwatch** 
- You use AWS Cost Explorer \(doesn't support attributing costs to multiple tenants\) and AWS Cost and Usage Reports to generate some reports and export it to S3 bucket
- You can use Athena and QuickSight to aggregate the reports and visualize it
- Options for cost allocations are not ideal, if we are hosting multi\-tenancy application it might not be easy to allocate cost of shared databases traffic etc.
- In general, first step is to measure tenant activity, and come up with the price for specific actions, which is calculated from cost that we can allocate and proportional part of shared costs 

### Cost allocation tags

- You can configure special tags which influence the Billing report and allows you to allocate costs to specific consumers
- allocation tags types
    - AWS generated tags 
    - user\-defined tags
- AWS generated tags requires activation

### Application Cost profiler 

- Enables you to allocate cost to specific tenant
- Requires creating an s3 bucket which Cost Profiler will use
- It only allows you to allocate time of resource usage by tenant

## Organizations

- Organizations let you group **multiple AWS accounts**
- Organization consists **root account** and possibly **multiple Organization units**, where everyone can group **multiple AWS accounts**
- You can manage **billing globally**
- You can manage **policies globally** in **organization**
- It **reduces costs** – in most cases **more you use, less you pay, so by grouping multiple accounts** volume of services usage gets sum up 
- You shouldn’t set up any resources for root account
- You can use **Service Control Policies** to **enable/disable** whole **services for specific Organization Units**
- **Accounts can be added to organizations by sending initation**
- **When you want to move account from one org to another you need to remove it from the first one and send invitation�**�
- **For moving master account to new organization, old one needs to be deleted first**
- **Organizations can be set up in “all features” or “consolidated billing” modes**

## RAM \- Resource Access Manager 

- **You can create resource centrally and share it with multiple different AWS accounts** 
- Supported services 
    - App Mesh 
    - Aurora 
    - CodeBuild 
    - EC2 
    - EC2 Image builder 
    - License manager 
    - Resource groups 
    - Route53 

## IAM

### ARN 

- arn:partition:service:region:account\-id:resource 
- arn:partition:service:region:account\-id:resource\_type:resource 
- arn:partition:service:region:account\-id:resource:qualifier 
- arn:partition:service:region:account\-id:resource\_type:resource:qualifier 
    - Partition – usually aws but can be different \(aws\-cn = AWS china\) 
    - Some parameters can be omitted 
    - Wildcards can be used 
- Example \- arn:aws:iam::123456789012:user

### Features 

- MFA 
- Identity Federation \- AD, Linkedin, Facebook 
- Password rotation 
- PCI DSS \(Payment Card Industry Data Security Standard\) Compliant 

### Structure

- **Users**
- **Groups** – for grouping users, users inherit permissions from the group 
- **Policies**
    - **json documents** defining what can be done on specific resource 
    - **Identity policy** – permission – can be **attached to resource group or role** 
    - **Resource policy** – **can be attached to the resource**, can define who and what actions can be made 
    - **Policies** do not have effect until are assigned to resource or user 
    - Every policy consists list of statements where each statement is related to some AWS API request 
        - Allow/Deny 
        - Action 
        - Resource \(ARN\)
    - There are **AWS managed policies** which are created in AWS out of the box 
    - You can define **inline policy directly in roles** 
    - **Everything is denied by default**  
- **Permission boundaries** – limits what are maximum permissions that can be attached to user or role 
- **Roles** 
    - IAM identity which can be be assigned with some permissions
    - Role be assigned to some AWS resource 
    - **To attach an IAM role to an instance that has no role**, the instance has to be in the **stopped** or **running state**.  
    - **To replace the IAM role** on an **instance** that **already has an attached IAM role**, **the instance must be in the running state**. 

### **Other** 

- You can generate **sign in link** after creating account for first logging 
- There is **root account** where the story begins 
- IAM service is global 
- **Access key id** and **Secret access key** are like user and password for programmatic access to the cloud 
- User do not have any permissions when created 
- You can utilize tags to make access to make permission management easier 

---

# User Directory Services

## Directory Service  

- Connecting **AWS resources** with **on\-premises Microsoft AD** 
- **Standalone directory** in the cloud 
- **SSO to any domain\-joined EC2 instance** 
- **AD specifics** 
    - **Hierarchical** – trees and forests 
    - **Group policies** 
    - Based on **LDAP** and **DNS** 
    - **Kerberos, LDAP** and **NTL authentication** 
    - **Highly available** 
- **AWS directory** provides **AD domain controllers \(DCs\)** in **HA mode** in **multiple AZs** 
- **DCs are not shared** with any other users of the cloud 
- You **can extend this AD to on\-premise machines** with **AD Trust�**� 
- Automated patching, Snapshot and restore  
- **VPN or Direct Connect connection required** 

### **AD connector** 

- **Connecting cloud auth** with **local AD** 
- On\-premise users can **log to the cloud with AD credentials** 
- Joining EC2 instance to existing AD 

### **Simple AD** 

- **Basic AD features** 
- **Small – 500 users** 
- **Large \- 5000 users** 
- **Do not support AD trust�**� 
- Works with **LDAP** for **linux machines�**� 

### **Cloud directory** 

- **AWS native directory** 
- **Fully managed** 

## **AWS Cognito** 

- **Managed directory for SaaS** 
- **Sign\-up** and **sign\-in** capabilities 
- **Integrates with social media** identities 
- **Web identity federation** – gives user access after successful authentication with web\-based identity provider like Amazon, Facebook or Google \(user gets auth code from Web ID provider, which can be traded for temporary AWS security credentials\)
- Cognito is web federation serice in AWS
    - **Sign\-up and sign\-in**
    - **Access for quests**
    - Acts as an **Identity broker** between your **application** and **Web ID** providers
        - After auth gives you **temp credentials** which are **mapped with some IAM role�**�
    - User pools 
        - **User Directory** managing sign in and sign up
        - Can **store the credentials** or **interface with some WEB ID provider**
        - Authentication generates JWT token
    - Identity pools
        - Provides temporary AWS credentials to AWS services like S3 or DynamoDB
    - Flow
        - **Users logins with facebook \(or something else\)** when user is **authenticated based on data in User pool�**�
        - **Cognito returns JWT token**
        - **User sends JWT token to identity pool** so **temporary AWS credentials** are **send to the client**
    - Cognito tracks associations between user identity and various devices used by the user
    - Cognito can be uses to **push updates and synchronize user data across  multiple devices**
    - Cognito **uses SNS** to send notifications to all devices associated with a given user
    - **SAML** support
    - **OIDC** support

### **AWS SSO** 

- You can **login to AWS** with **Active Directory credentials** 
- You can use AWS SSO to login to Apps like GSuite or office360 
- Based on **SAML**

---

# Networking

## Route53

Route53 name – based on DNS port number 

### DNS 101

- Domains in Global DNS system are controlled by **IANA** \(Internet Assigned Numbers Authority\)
- Domains are registered to top level domain \(.com, .[co.uk](http://co.uk) etc.\) by domain registrar through InterNIC which is a service of ICANN who is knowns as the WhoIS database
- Records
    - **SOA** \(Start of Authority\) – name of the server that supplied the data for zone, administrator of the zone, current version of data file, default TTL
    - **NS** \(Nameserver\) – Nameserver contains authorities DNS record, to level domain servers are redirecting traffic DNS to this server
    - **A** \(Address\)
    - **CNAME** \(Canonical Name\) \- aliasing one domain name with another
    - Alias record \(AWS specific\) \- let you map Domain to ELB, CloudFront or S3
        - CNAME cannot be used for naked domain names
        - “Route 53 allows you to create an Alias record at the top node of a DNS namespace \(zone apex\)”
    - **MX** – mail
    - **PTR** record opposite A record 

### Routing policy

- **Simple Routing**
    - only one record, with one or more addresses
    - Different addresses are returned in random order \(client can load balance or failover\)
- **Weighted Routing�**�
    - You can split traffic to different regions \(IPs\)
    - You can set weight
    - All weights are added so weight / sum\(Weights\) = percentage of traffic 
- **Latency\-based routing**
    - Answer to request is based on latency from DNS server to target servver 
- **Failover routing**
    - You can define active and passive target
    - When active fails, switch to passive is made
- **Geolocation routing**
    - DNS resolution is made based on geolocation of ip of requester 
- **Geoproximity routing**
    - Routing is based on geographic location of you users and requested resources
    - You can set **“Bias”** which **expands or shrinks geographic region from which traffic is routed**
    - **You have to use “Route 53 traffic flow”**
    - You can set up multiple different rules based on geo locations
- Multivalued answer routing
    - **Like simple routing but enables you to set up health checks for each record set**

### **Other**

- You can register domains directly in Route63 – **up to 50 domains**, you can extend it after contacting support
    **ELB** do not have static IP, you have to always use its DNS name
- **You can set up health checks** – fail removes it from routing until it will be healthy again
- **You can set up notifications for health checks**

## VPC \(Virtual Private Cloud\)

![Untitled-1.png](image/Untitled-1.png)

- You can select IP ranges and create subnets \(subnet belongs to only one AZ\)
- **Elastic IP** – **public IP address** which **can be assigned to EC2**
- **Routing tables**
    - There is default route in routing table which allows you to communicate between AZ within VPC
- **NACLs**
- **Internet gateways** \(only one per VPC\)
- **Security groups**
- VPC reserved addresses
    - **[10.0.0.0](http://10.0.0.0) \- Network address**
    - **[10.0.0.1](http://10.0.0.1) \- VPC Router**
    - **[10.0.0.2](http://10.0.0.2) \- DNS server \(also present as second address in each subnet\)**
    - **[10.0.0.3](http://10.0.0.3) \- Reserved by AWS for future use?**
    - **[10.0.0.255](http://10.0.0.255) \- Network broadcast address \(broadcast is not working\)**
- Public subnet
    - A subnet which has **at least one route to IGW**
- You can have **5 VPCs on AWS account** by default

### Default VPC 

- **all instances in default VPC have internet access**
- **All instances in default VPC both private and public IPs**
    - When creating VPC following resources are created 
    - **Security group**
    - **Network ACL**
    - **Route table**

### VPC peering

- Allows to connect to private VPCs via direct network route
- **It can be done between different accounts or different regions**
- **VPC has to be peered directly it is not possible to root traffic true third VP**C

### Nat Instance vs Nat Gateway

- NAT instance
    - NAT Instance is EC2 instance which is performing NAT Gateway role
    - **You need to disable source/destination check**
    - It’s not scalable automatically
    - **NAT instance must be in public subnet**
    - HA and autoscaling must be scripted
- NAT Gateway
    - It is managed NAT server 
    - **Redundant in AZ**
    - Scalable from 5 Gbps to 45 GBps
    - **If having resources in multiple AZ it is good idea to have one NAT Gateway in each of AZ**
    - Ephermal ports range 1024\-65535

### Network Access control list vs security groups

- Security groups
    - **All outbound traffic is allowed by default \(?\)**
    - **All inbound traffic is allowed by default \(?\)**
    - **Stateful – Any changes applied to incoming rule will be automatically applied to the outgoing rule**
- Network ACL
    - It’s recommended to set prio to rule by increment of 100
    - Rules with lower number has priority
    - **Network ACL** are evaluated **before security groups**
    - **Each subnet needs to have its ACL**
    - **Network ACL can be associated with multiple subnets**
    - **Subnet can be associated only with one ACL**
    - **Stateless – any changes applied to incoming rule wont be automatically applied to outgoing rule**
    - **Blacklisting IPS**
        - You can use **NACL** to block malicious IPs
        - With ALB **NACL** needs to be set up in **ALB subnet**, and via security group we can configure that only traffic from ALB is possible \(with ALB original connection is terminated on ALB so target machine doesn't know origin IP\)
        - With NLB you can simply use the NACL
- Ephermal ports –port used only for duration of single communication session

### VPC Flow logs

- **Logs from IP traffic**
- Stored in Amazon CloudWatch Logs 
- **Levels**
    - **VPC�**�
    - **Subnet**
    - **Network interface�**�
- You cannot create flow log with VPC from another account \(even if peered\)
- Exclusions
    - **Traffic to Amazon DNS server**
    - **Traffic generated by Windows instance for Amazon Windows license activation**
    - **Traffic to and form [169.254.169.254](http://169.254.169.254) for instance metadata**
    - **DHCP traffic**
    - **Traffic to reserved IP address for the default VPC router**

### AWS Network Costs 

- **Inbound connection are free**
- **Connection** inside AWS \(using private IP\) **within single AZ are free**
- Traffic **between AZs,** **~$0.01 per GB**
- Traffic **through internet** \(with public addresses\) **~$0.02 withing single Region**
- Traffic between regions also $0.02

### Egress\-only internet gateway

- **allow IPv6 based traffic** within a VPC to **access the internet**
- denying any internet\-based resources to connection back into the VPC

### Bastion hosts

- ### Special purpose computer in DMZ, used to handle acccess from untrusted networks and computers \(all other functions should be limited\)
- HA with Bastion hosts
    - You can set up **network load balancer** before your bastion instances to have it in **active/active** mode
    - You can also set up in **active/passive**, by simply creating bastion host in **autoscaling group in \(1 min, 1 max instances\)**

## Direct connect

- **Direct private connection between your datacenter and AWS**
- \(Not using VPN by default\)
- ![Untitled-2.png](image/Untitled-2.png)
    Better bandwidth, lower network costs
- Setting up Direct Connect \(Memorize it\!\)
    - Create virtual interface in the Direct Connect console
    - VPC console \-\> VPN connection \-\> Customer Gateway
    - Create Virtual private gateway
    - Attach Virtual Private Gateway to desired VPC
    - Select VPN Connections and create new VPN Connection
    - Select VPG and Customer Gateway
    - Once the VPN is available,, set up the VPN on the customer gateway or firewall 

### Security groups

- Changes in security groups takes effect immediately
- **All inbound traffic is blocked by default**
- **All outbound is allowed by default**
- **You can have multiple security groups added to ec2 instance**
- You cannot block specific IP address using security groups
- **You can specify only allow groups not deny**

## Global Accelerator

- **GA directs traffic to optimal endpoints over the AWS global network�**�
- User is getting through **Edge locations** through AWS Global Accelerator which **based on proximity, health and endpoint weight** choosing **optimal endpoint group,** from which traffic is routed to the true endpoint \(ALB, NLB, EC2\)
    By default, you are getting **2 static IP public addresses�**�
- **It lowers the cost of data traffic�**�

### Architecture

- **Static IP address** – by default [1.2.3.4](http://1.2.3.4) and [5.6.7.8](http://5.6.7.8)
- **Accelerator**
- **Network ZONE** – like AZ, gives you two private static addresses
    **DNS Name**
- **Listener**
    - inbounds connections from clients to Gloval Accelerator, based on port \(or port range\) and protocol that you configure, both TCP and UDP are supported
    - Each listener has one or more endpoint groups associated with it and traffic is forwarder to endpoint in one of the groups
    - **Client affinity** – you can set up listener for specific ip address or range
- Endpoint Group
    - Each **endpoint group** is associated with a **specific AWS Region**
    - Endpoint groups include one or more endpoints in the region
    - Traffic dial – you can reduce or interface the percentage of traffic that would be otherwise directed to an endpoint group
        - Useful for performance testing or blue / green deployment
- Endpoint
    - **ALB, NLB, EC2, Elastic IP address�**�
    - **Endpoint can have their weights**
    - ALB can be internet facing or internal
    - **VPC Endpoints**
- **VPC endpoint enables you to connect you VPC with AWS services endpoints without using Direct Connect, VPN, NAT** or internet gateway so such traffic wouldn’t exit AWS network
- Endpoint are virtual devices
- Endpoint are horizontally scaled 
- Between regions VPC needs to be peered in order to VPC endpoint to work
- **Types**
    - **Interface endpoint** – **ENI** with **private IP address**, serves as an entry point for traffic destined to a supported service
    - **Gateway endpoint** – like **NAT gateway,** supported only for **DynamoDB** and **S3**

## AWS Private link

- Secure private connectivity for services between separate VPCs
- VPC Endpoint \-\> VPC Service Endpoint \-\> NLB \-\> Service within VPC \( can be used within one region\)
- Let’s you **to share services in one VPC with multiple different VPCs**
- **Easier to do that** on big scale than by **VPC peering**
- **More secure than opening VPC** to internet \(then you have to harden it\)
- Requires **NLB on service VPC** and **ENI on the customer VPC**

## AWS Transit Gateway

- **Single point entry point for internet traffic**
- You can **handle traffic from/to VPN endpoints, VPC, direct Connect** etc.
- Works on **hub\-and\-spoke** model
    - There is a central component \(hub\) used to orchestrate all traffic between mutliple sources and destinations
- **Regional resource**, but **can be configured across multiple regions**
- Can be configured across multiple AWS accounts with RAM \(Resource Access Manager\)
- Route tables can limit how VPCs talk to one another
- **Support IP multicast**
- **...**

## VPN CloudHub

- Enables you to **configure connection between multiple datacenters** and **VPCs** with **VPN \(datacenters can also connect with each other\)**
- **All traffic encrypted�**�
    Hub and spoke 
- Architecture
    - Connection is made by setting up VPN tunnels between on\-premise data centers and VGW \(Cirtual Gatyeway\)
    - BGP Peering is configured between customer router & VGW  using unique BGP VGW at each location
    - VGW is retrieving prefixes from each location and its re\-advertising it to other peers
    - Ip addresses in all locations cannot overlap

## **AWS Site\-to\-Site VPN**

Enables to configure VVPN tunel to client network

- VGW – virtual gateway
    - ECMP \(Equal Cost Multi\-Path\) which boost performance is enabled by default \(needs to be enabled on on\-premise datacenter side\) 

## AWS Loadbalancers

- **Application load balancer�**�
    - based on **HTTP** and **HTTPs traffic \(Layer 7\)**
    - You need to create **target group** that can be associated with load balancer
    - You can set up **different routes** based on **http headers, paths etc.**
    - **Sticky sessions** supported **on target group level**
- Network load balancer 
    - **TCP traffic**, **Layer 4, high performance, low latency**
    - **Can terminate TLS connection reducing the load on EC2 instance�**�
    - **You can configure security policy for specifing cypher and protocols for TLS handshake**
- Classic load balancer
    - Support both layer 7 and layer 4 load balancing
    - Supports **X\-Forwarded** – original IP of the client can be saved in this header on load balancer
    - **Supports sticky sessions** – request will be send to specific target during the sessions
    - If application is not responding LB will respond with **504** http error
    - **Cross zone load balancing** – traffic will be load balanced across zones evenly
    - **Connection draining period** – time when traffic can still flow before removing vm from load balancing targets
- **Health checks**
    - **Response timeout**
    - **HealthCheck interval**
    - **Unhealthy threshold**
    - **Healthy threshold**
- Other
    - Load balancers are always associated with DNS name, you are never provided with IP address
    - There are two statuses of instance in context of load balancing – InService and OutOfService
    - You need to have **at least two subnets** in order to create load balancer \(?\)
    - ALB needs to be deployed at least to two different AZs

## WAF \- Application \(L7\) level firewall 

- You can allow connection or respond with 403
- There are 3 behaviors
    - **Allow** all expect specified
    - **Block** all except specified
    - **Count** request that match properties you specify
- Rules can be based on
    - **Sender IP address**
    - **IP Geo location**
    - **Request headers**
    - **Regexp�**�
    - **Length of request**
    - **SQL injection code presence**
    - **XSS code presence�**�
    - **Query string parameters**
- WAF is commonly used to blacklist malicious IPs

## AWS firewall manager

- L**et you define WAF rules for ALB, API Gateway and CloudFront distributions over whole AWS organization**
- Let you define **AWS Shield Advanced** protections for **ALB, ELB Classic, EIP, CloudFront**
- Enable security groups for **EC2** and **ENIs**

---

# Compute

## EC2

### Hypervisors

- **XEN**
- **Nitro** \(Developed by AWS\)

### Windows instances

- Windows instances are billed by full hours
- Storage is billed evven if instance is idle

### Pricing models

- **On\-demand** – no commitment, hourly rate
- **Reserved** – 1 or 3 years Terms, significant discount
    - **Standard** – up to 75% off, you have to pick type of instance for whole contract
    - **Convertible** – up to 53% off, you are reserving resources but you can use different types of EC2 machines
    - **Scheduled** – you are getting your machines only in specified time window
- **Spot** – you decide the price that you want to pay, if current price \(which is calculated based on available cloud compute resources\) is lower the you are getting your machine, otherwise it’s
    - You are not charge for partial hours where AWS terminated your instance
    - You are charged for partial hours where you terminated your instance
- Dedicated host \- you can use your own license and get physical server instance \(on\-demand or Reserved\)
    - Tenancy attribute:
        - **Default** – hosted on shared hardware
        - **Dedicated** – hosted on single\-tenant hardware
        - **Host** – hosted on isolate server with configuration which you control
        - You **cannot change** this attribute **between host/dedicated and default** after launching instance 
- RI Utilization – show how much reserved instances are utilized
- RI coverage – shows how much of all ec2 is handled by resered instances 
- **Saving plan** 
    - you can **define** amount of usage in **$/hour** in **1 or 3 years** term
    - You are **billed lower rate up to this commitment**
    - Works for **Lambda, EC2, Fargate**

### Instances types

![Untitled.png](image/Untitled.png)

### **Network interfaces**

- ENI – virtual network card
    - Gets at least one private IPv4 address
    - One or more IPv6 addresses
        Can get one public IPv4 address
    - Gets MAC address
    - Create managed network \(?\)
    - Use network and security appliances in your VPC \(?\)
    - Create dual homed instances with workloads/roles on different subnets\(?\) 
    - Create a low\-budget, high\-availability solution
- **EN – Enhanced networking**, uses single root I/O virtualization for high performance
    - **Higher I/O** performance
    - **Lower CPU** utilization
    - **Higher bandwidth, lower latencies**
    - No additional charge
    - Types
        - ENA  \(preferred\)
            - Up to 100 GBps
        - Virtual function \(Intel 82599\) \- up to 10 gbps
- **Elastic Fabric Adapter \(EFA\)** – network device for High performance computing \(HPC\), can be attached to EC2, mostly for machine learning purposes
    - **Lowest latency**, **highest throughput**
    - Machine learning applications can connect to it **bypassing the OS kernel�**� \(only on linux\)
    - You cannot move it to antoher AZ after creation

### Spot instances

- Up to 90% compared to On\-Demand
- **Spot blocks�**�
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

### **Spot fleets**

- **Collection of spot and \(optionally on\-demand\) instances**
- You can set up different launch pools for different EC2 types, AZs, OS types
- **Strategies**
    - **CapacityOptimizes** – stop instance come from the pool with optimal capacity for the number of instances launching
    - **Diversified** – distributed across all polls
    - **LowestPrice** \(default\) \- stop instance come from the pool with the lowest price 
    - **InstancePoolsToUseCount** – The spot Instances are distributed across the number of Spot Instance pools you specify. Used together with lowestPrice

### Instance Metadata

- You can curl **[169.254.169.254/latest/user\-data](http://169.254.169.254/latest/user-data)**to get boot script for the instance
- **[169.254.169.254/latest/meta\-data/](http://169.254.169.254/latest/meta-data/)\<meta\-data\-name\>** \- to get multiple different metadata about the instance

### Autoscalling

- **Basic terms**
    - **Groups** – group of components being scaled \(eg. applications, DBs\)
    - **Configuration templates** – configuration used to create EC2 instances \(AMI ID, instance type, key pair, security groups block device etc. \)
    - **Scaling options** – rules on how and when autoscaling is performed
- Scaling options
    - **Maintain instance levels** – if health check fail, instance is terminated and new one is created
    - **Scale manually**
    - Scale **based on schedule**
    - Scale **based on demand** – scaling based on **CPU utilization**, **Networks bytes in/out**, **ALB request** per target
    - Use **predictive scaling** – predicted automatically based on previous performance
- Other
    - Scalling Cooldown timer – period of time where instance will be off from work after spin off

### Placement groups

- Types
    - **Clustered placement groups**
        - **Grouping in single AZ**
        - **Low latency, high throughput**
        - **EC2 instances within clustered placement group should be homogenous**
    - **Spread placement group**
        - **Run on different hardware**
        - **Can be in different AZ**
        - **Individual critical EC2 instances**
    - Partition placement group 
        - **Multiple servers in partitions**
        - **Partitions needs to be in different racks**
        - Multiple EC2 instances \(e.g. HDFS, HBase, Cassandra\)
        - Only specific types of EC2 can be put into Placement group
            - Compute Optimized
            - GPU
            - Memory Optimized
            - Storage Optimized
            - **You cant merge placement groups**
- **You can move existing instance into placement group�**�
- **One machine can be only in one placement group**
- **Placement group cannot span over multiple regions**
- Single EC2 instance can be hosted in only one placement group at the time
- **Spread placement group** can have **maximum 7 instance per AZ**

# EBS

- **EC2 root disk can be only**
    - **General purpose SSD**
    - **Provisioned IOPS SSD**
    - **Magnetic**

**Disk types**

    - **Cold HDD – 250 IPOS / 250 MiB/s**
    - **Throughput optimized HDD – 500 IOPS / 500 MiB/s**
    - **General purpose SSD \- 16 000 IOPS / 1000 Mib/s**
    - **Provisioned IOPS SSD \- 256 00 IOPS / 4000 MiB/s**
    - **Magnetic – 40\-200 IOPS**

### **AWS Data Lifecycle**

- **Tool for automation of deleteion, retention and creation of EBS snaposhots**

### **Snapshots**

- **Snapshot of EBS volumes are incremental**
    - **But every snapshot consist all the data needed to restore ebs \(?\)**
- **To make a snapshot of the root drive you should stop an instance**

### AMIs

- You can't delete a snapshot of the root device of an EBS volume used by a registered AMI. You must first deregister the AMI before you can delete the snapshot
-  AMI virtualization types
    - PV
    - HVM \(much more ec2 instances support it\)
- AMIs types
    - EBS volume – created from EBS snapshot \(supported for most instances type\)
    - Instance store volume \(ephermal storage\) – created from the template stored on S3 \(supported by only couple instances types\)
        - All instance volumes has to be attached to EC2 during its creation
        - EC2 with instance store volumes cannot be stopped
        - In case of hypervisor failure all data will be lost

### EBS Hibernate

- **Allows you to perform EC2 hibernation – save the contents form RAM to EBS volume**
- Hibernated instance after being waked up retains its instance ID
- Boot is much faster, all processes are resumed
- Useful for workloads where processes cannot be interrupted or for services which are booting lot of time
- Uptime won’t be restarted
- **RAM** must be **less than 150 GB**
- **No more than 60 days of hibernation**
- Available only for On\-Demand and reserved instances 
- Not available for autoscalling group

### **Other**

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

## ElasticBeanstalk

- You are uploading code, while beanstalk is creating infrastructure, takes care of load balancing scaling and application health monitoring
- Supports also containers
- Configuration data can be kept on S3

## API gateway

- Expose HTTPS endpoints to define RESTful API
- Serverlessly connect to services like Lambda and DynamoDB
- Scale effortlessly
- **Throttle requests to prevent attacks**
    Track and control usage by API key
- Connect to CloudWatch to all request for monitoring
- Structure
    - API
        - Resources
            - HTTP method
            - Security
            - Target
            - Request and response transformations
            - Autogenerated or custom domain
- Can assign ACM certificate
- **API caching** – caching endpoint response for specified time \(TTL\)
- **CORS�**�
    - Same origin policy \- One page is allowed to get data from second page only if they have the same origin
        - Prevents XSS attacks
        - Enforced only by web browsers
    - CORS – Cross origin resource sharing 
        - Browsers set HTTP OPTIONS
        - Server returns with list of domains that are approved to GET this URL
        - Kinesis
- **Streaming data** – data are streamed continuously by thousands of data sources

## Lambda

- **Lambda**
    - **Lambda** takes care about provisioning and managing the servers that you use to run the code, patching os management, scaling
    - Ways of using **lambdas**
        - **Event driven compute service** \(like changes in **S3 buckets** or **DynamoDB**\)
        - Serving **HTTP request** using **AWS API gateway**
    - **Pricing**
        - **Num of requests** \- First 1 million free, 0.20 per million requests after that
        - **Duration** \(time from code begins executing to termination rounded nearest 100ms\) 0.00001667 for every GB/s
    - **Hyperthreading** support
        Lambda scales out \(not up\) \- **1 event = 1 function**
    - **Synchronous triggers**
        - ALB
        - Cognito
        - Lex
        - Alexa
        - API Gateway
        - CloudFront 
        - Kinesis Data Firehose 
    - Asynchronous triggers 
        - S3

## ECS

- ECS \- managed orchestration service
- You can create clusters to manage fleets of container deployments
- ECS manages **EC2** or **Fargate** instances 
- You can **define CPU** and **memory requirements**
- **Basic monitoring** available
- You are paying only for **compute resources**

### Components

- **Cluster** – logical collection of ECS resources –ECS **EC2** instances or **Fargate**
- **Task** – **Single running copy of any container** defined by task definition
- **Task Definition** – **Definition of container** \(or **multiple containers**\)
- **Service** – Allows **task definitions** to be **scaled by adding tasks**, **defining minimum** and **maximum values**
- **Container Definition** – i**nside task definition**, it defines the individual  containers a task uses, controls **CPU, memory and port mappings**
- **Registry** – Storage for container images 

### Integration with ELB

- Works with ALB NLB and CLB

### Security

- If role is assigned to EC2 then all containers inherit it
- Role can be assigned to the task \- preffered

## Fargate

- **Serverless container engine**
- Working with **ECS** and **EKS**
- No need to provision servers
- **Payment for resources** used by application
- **Each workload** runs in its **own kernel**
- Isolation and security
- **No GPU support**

## EKS

- Elastic K8S service

## ECR

- **Managed docker registry**
- **HA** – multi AZ 
- **Integrated with IAM**
- **Paying** for **storage** and **data transfer**
- **Can scan images for OS and runtimes vulnerabilities**

---

# Storage

## DynamoDB

### **Core concepts**

- There are **tables**, **items** and **attributes**
- **Attributes** can be **scalar** or **nested** \(up to **32 levels deep**\)
- Every item needs to have **primary key**
    - **Primary key** can be **single value** \- Partition Key \- then primary key must be unique
    - **Composite Primary Key** \-  **Partition Key and sort key** then Partition key don't need to be unique
    - Primary key is always subject of hash function which determines where exactly data is stored
- Hash function is deciding where to put specific item
- If  using simple Primary key data are not kept in sorted manner
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
    - **Latest restorable** – **five minutes in the past**

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
                - for transactional ready you need 2 request units  for 4 kb size item
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
- **DAX** – **DynamoDB accelerator**
    - **Boost latency** from **milliseconds to microseconds**
    - No **cache integration** on **client side** is needed
    - **Read and write support**
    - Can be used for HA

### **Global Tables**

- **Managed Multi master Multi region replication**
- **Based on** **DB streams**
- Used for **DR** and **HA** \(multi region\)
- **Replication between regions** under one second

## S3 \- Simple Storage Service

### General informations

- Object storage \(files\)
- **Max size** of file **5TB**
- Unlimited storage 
- Updates to objects make some time to propagate
- Files are stored in **buckets** which has **got globally unique names�**� 
- Every **bucket gets it’s own DNS address** 
- After uploading file to the **bucket** you will get **200 HTTP response�**� 
- **Resource owner – AWS account** that **creates** Amazon S3 **bucket** and **objects** 
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

### **Tiers** – storage classes 

- **S3 standard** – high availability and durability, can survive losing of 2 databases 
- **Reduced redundancy \-** designed for noncritical, reproducible data that can be stored with less redundancy than the S3 Standard storage class, frequent access 
- **IA** \- for non\-frequent rapid access, lower storage fee but additional fee for data retrieval  
- **One zone – IA** – IA but in only one AZ, low cost but higher risk of data loss 
- **Intelligent tiering** – Choosing tier based on machine learning algorithm which is analyzing how often specific data are retrieved  
- **Glacier** – for archiving data, super cheap, but long retrieval time \(from minutes to hours\), retrieval fee 
- **Glacier Deep Archive** – Lowest cost, retrieval time configurable in multiple hours, retrieval fee 
- **Glacier Archive retrieval options** 
    - **Expedited** – for files up to 250 mb, available within **5 minutes** 
    - **Standard** – available within 3\-5 hours 
    - **Bulk** – petabytes 5\-12 hours 

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
    - **Bucket policies�**�
    - **Object policies**
    - **IAM policies** \(for users and groups\)

### Billing

- **Charging based on**
    - **Storage**
    - **Requests**
    - **Storage management pricing�**�
    - **Data transfer pricing**
    - **Transfer acceleration** \(if enabled\)
        - feature which uses CloudFront to speed up transfer to end users
        - Works for files up to 1Gb
    - **Cross region replication pricing** \(if enabled\)
- **Encryption**
    - **In transit – SSL/TLS** \- to secure transit
    - **On rest – encryption of storage data**
        - **S3 managed keys – SSE\-S3**
        - **AWS Key management service keys – SSE\-KMS**
        - **Customer Provided keys – SSE\-C**
        - **Client\-side encryption** – data can be encrypted on client side and uploaded

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
    - Replication – when replication failed or exceeds 15m

## AWS Data Sync

- Allows you to **sync big amounts of data from your local data center with AWS**
- Uses the **agent installed on source machine**
- Can be used with **NFS** or **SMB systems**
- **Replication** can be done **hourly, daily** or **weekly**
- Can be used to sync with **EFS**
- Can be uploaded to **S3, EFS, FSx**

### Other features

- **MFA delete�**�
- Object lock
    - For fulfilling WORM \(Write once Read many regulations\) or adding additional layer of security
    - **Retention period** – period when file cannot be deleted
    - Modes
        - **Governance mode** – let you grant possibility of deleting file only for specific users
        - **Compliance lock** – nobody \(even root user\) cannot delete file and retention period cannot be shorten
        - **Legal hold** – object cannot be deleted until legal hold is active 
- **Glacier Vault lock – just like s3 object lock \(but simpler\)**
- **Access logs available** 
    - can be written to some bucket 
    - You can then export it to another account by replication feature
- **Sharing S3 bucket**
    **S3 Select** and **Glacier Select** – allows you to **query** only a part of data **from an object stored on S3 \(eg. Csv file, json\)**
    - By **Bucket policy** \(only programmatically\)
    - By **Bucket ACL**
    - **Cross Account IAM Roles**

## Aurora

- **5 times faster than MySQL and 3 times faster than PostgreSQL at lower price**
- **Start at 10 Gb and scales with additional 10 Gb up to 64TB**
- Compute resources scalable to 32 vCPU and 244  GB RAM
- **2 copies of data are in each availability zone in minimum 3 zones = 6 copies of data**

### **High Availability**

Replicas types

- Aurora \(15\)
- MySQL \(5\)
- PostgreSQL \(1\)

Replicas additional information

- **MySQL replicas impacts aurora performance**
- MySQL replica can be in other region while aurora only in the same region as primary instance
- **Aurora replica support automatic failover**
- **MySQL replica can support different schema or data**
- Every replica has its own DNS address

### Backups

- Enabled by default
- **Do not impact performance**
- **Snapshot can be taken \(also do not impact performance\)**
- Snapshots can be taken from another AWS account

### Aurora Serverless

- **There is also Serverless version, which is scaling automatically based on needs**
- Billable on per invocation basis

## RDS

Relational databases available on AWS, available engines:

- **MS SQL**
- **Oracle**
- **MySQL**
- **PostgreSQL**
- **Aurora**
- **MariaDB**

### **Multi\-AZ Disaster recovery�**�

- **Second RDS replicas** are deployed **in different AZ**
    **Active / Active**, switch by DNS
- When Primary AZ will go down then DNS is automatically switched to second AZ
- AWS is handling **replication automatically \(snchronously\)**
- Available for all database engines besides **Aurora** which is **Fault tolerant by design**
- You can **force failover by rebooting primary instance**
- **When enabled, backups are bade from secondary db so I/o for primamry is not interupted�**�

### **Read Replicas** – **performance boost**

- **Different URL** for **read replicas cluster**
- You can architect your system to write to write replica and read for read replicas
- Works for all db engines **besides MS SQL**
    New **data are replicated asynchronously** to all read replicas
- **Automated backup has to be enabled**
- You can have **up to 5 read replicas**
- You can have **read replicas of read replicas** \(?\!\)
- Each read replicas has it own DNS endpoint
- **Read replicas** can be in **multiple AZs** as well as **different regions**
- **Read replicas** can be used for **MultiAZ databases**
- Every **read replica can be promoted to autonomous Database** \(It breaks the replication\)
- You can create **Aurora Read replicas for other Database engines** \(can be also used to migrate your db to Aurora\)
- There might be some **propagation lag**

### Backups

- **Automated backups**
    - You can set **retention period up to 35 days**
    - **RDS** is doing **daily backups \(full snapshot of database storage volume\) plus transaction logs in eery 5 minutes**
    - When restoring RDS is choosing latest daily backup and applying transactions from transaction log on top of it
    - This let you to **restore** the database with **precision to seconds**
    - **Enabled by default**
    - **Backups stored in S3** \(storage is free of size of your database\)
    - **During backup window storage I/O might be suspended**
    - **Automated backups are deleted during deletion of RDS instance**
- **Snapshots**
    - **Full backup of the database**
    - It's **not deleted** when **deleting RDS instance**
- **Restore**
    - **Restore** is always made on **new RDS instance** with new DNS endpoint

### HA

- RDS Standby \(Secondary\) instances are only for failover, for increasing read performance you need to use read replicas

### **Encryption**

- Done by **KMS**
- **Encrypting storage data, automated backups** and **snapshots**
- If you want to encrypt unencrypted database you need to copy its snapshot encrypt it and create DB from it 

### **Other**

- RDS is run on **VMs**, but **you cannot ssh to them**
- RDS is **not serverless** \(**Aurora Serverless is an exception**\)
- Aurora can invoke lambda functions
- You can shard your data into different DBS for write performance
- YOu can configure RDS database by creating parameters group and attaching it to DB instance 

## EFS

- EFS volumes can be **shared between EC2s**
- **Filesystem grows automatically**
- There is **bursting / provisioned throughput** options
    **EFS Lifecycle –** if data is **not used fox x days** it can be moved to cheaper **Infrequent use storage class**
- Performance modes – **General Purpose** and **Max I/O**
- Encryption can be enabled with KMS
- Communication is made over **NFS** \(v4\) – Security groups must reflect that
- You can enable TLS transport encryption during the mount
- You pay only for storage you use
- It scales to petabytes
- Data is stored in multiple AZ
- Read after write consistency
- EFS mount helper – tool for mounting EFS volumes can also enable transit encryption by creating tunel

## Amazon FSx

- **Windows File Server**
    - **Storage for Windows OS**
    - Built on Windows Server
    - **Communication is over SMB**
    - **Support Active Directory users, ACLs, Groups, Security Policies**
    - **Support Distributed File System namespaces and replication**
- **FSx for Lustre**
    - **Supports Lustre filesystem \(hundreds gigabytes per second of throughput, millions IOPS, sub\-millisecond latencies\)**
    - Use cases:
        - Compute\-intensive workloads
        - High\-performance computing
        - Machine learning
        - Media data processing
        - Electronic design automation

---

# Messaging

## Event processing patters 

Event\-Driven Architecture \- Pub/sub architecture

**Dead\-letter queue –** Queue of letters that hasn’t been received by any consumer 

- SNS – if message will fail to be delivered to the target is put to DLQ
- **SQS** – when message exceed **maxRecieveCount** \(will be received but not deleted\) then it goes to DLQ
- Lambda – if lambda will raise an error during message processing, it will be **retried twice** and sent to DLQ \(SQS or SNS\)

## Fanout pattern 

![Untitled-3.png](image/Untitled-3.png)

## SQS

- **AWS Message queue**
- Message can contain up to **256 KB**
- Can be used to **queue writes** to the **database**
- Queue types
    - **Standard queues**
        - Unlimited number of transactions per second
        - Guarantee that **transaction** will be **delivered at least ones**
        - Occasionally **one copy of the message** might be delivered **out of order**
        - **Best performance**
    - **FIFO queue**
        - **Fifo enforced**
        - **Exactly\-once processing�**�
- **Pull\-based**
- Default retention period is 4 days
    Messages can be kept in queue from 1 minute to 14 days
- **Visibility timeout**
    - **after message is delivered is not being visible in the queue for some period of time**, if it is processed due end of that time its deleted, otherwise is visible again
    - Max 12h
- **SQS short polling** – pooling message from the queue, **if queue is empty then empty response is made�**�
- **SQS long polling** – r**esponse is not returned until message arrives** in the queue

## **AmazonMQ** 

Managed ActiveMQ service – JMS and MQTT compatible

## Simple WorkFlow Service

- **Coordinating work flow across distributed applications**
- Workflow execution can last **up to one year**
    Represent invocations of various processing steps in an application which can be performed by **executable code, web service calls, human actions, scripts**
- **Task oriented API** 
- **Task** is assigned only once and is **never duplicated** 
- **Actors**
    - **Workflow Starters**
    - **Deciders** – controlling the flow of activity tasks, if something has been finished or failed deciders choose what to do next 
    - **Activity workers** – carry out the activity tasks

## SNS – Simple notification service

- SNS allows you to push notifications to:
    - Google
        Apple
    - Fire OS
    - **Windows devices**
    - **Android Devices**
    - Baidu Cloud push 
    - **SMS**
    - **Email \- \(Doesn’t use SES directly\)**
    - **SQS�**�
    - **HTTP**
- Recipients can be grouped in **topics**
- Different recipient types can be grouped in one topic
- **Push\-based**

---

# Monitoring

# Cloudwatch

- **EC2 Host Level metrics**
    - **CPU**
    - **Network**
    - **Disk**
    - **Status Chech**
- **5 minutes interval by default**
- **You can turn on 1 minute interval**
- Dashboards can be regional or global
- CloudWatch events is a stream of events for state changes in AWS
- Data can be saved on S3
- By default **cloudwatch doesn’t know how much memory is used**
- By default cloudwatch doesn’t know what CPU is used for \(but knows how much is it used\)

## Cloudtrail

- Logs form calls to AWS
- When you enable it for all regions all future regions will be also included 
- Logs are encrypted by default
- There is data integrity feature when saving to S3

---

# Moving data

## DMS

- **Database migration service**
- Available for migration from and to EC2, on\-premise and RDS instances
- You can define all transformation logic
- Database is fully operational during data migration
- Migration can be done from one db engine to another
- **AWS Schema conversion tool** – can pre create target table automatiacally
- **Homogenous migration** – same databases
- **Heterogenous** migration – different databases – SCT needed

### DB Engines support

- Source
    - On prem
        - Oracle
        - MS SQL
        - MySQL
        - MariaDB
        - PostgreSQL
        - SAP
        - Mongo
        - DB2
    - Azure SQL
    - Amazon RDS
    - AWS S3
- Target
    - On prem
        - Oracle
        - MS SQL
        - MySQL
        - MariaDB
        - SAP
        - PostgreSQL
    - RDS
    - Redshift
    - DynamoDB
    - S3
    - Elasticsearch
    - Kinesis
    - DocumentDB

## Server Migration Service \(SMS\)

- Supports incremental replication of your on\-premises servers to AWS
- Backup tool, multi\-site strategy, DR tool 

## AWS Application Discovery Service 

- **Gathering data about local datacenter to plan the migration**
- You can install **AWS Application Discovery Agentless Connector** as a virtual Applicance on vmWare
- It’s building server utilization and dependency for on\-premises env
- All collected data are encrypted and retained
- You can export such data to CSV and estimate Total Cost of Ownership 
- It’s connected with **AWS Migration Hub**, when you can migrate discovered servers

### VM Import/Export

- Migrating on\-premise instances to EC2
- Can be used to create DR strategy on AWS or AWS second site

## AWS Datasync

- tool for migration data into cloud
- stores on S3, EFS, FSx for Windows
- Agent can be installed on on\-premise machine 
- Can use DirectConect

## Kinesis \(firehose\)

**S**treaming service in AWS

### **Streams**

- can receive data from different sources
- Data areas stored in shards \(every shard can have different purpose\)
    Data are stored for 24g – 7d retention
    - Shard has 5 transaction per second for reads
    - 2MB per second
    - Up to 1000 records per seconds for writes
    - Total data write rate of 1MB per second
    - Capacity of the stream = sum of capacity of all shards 
- Data can be streamed to some consumers

### **Firehose�**�

- Elements
    - **Delivery streams**
    - **Records of data**
    - **destinations**
- Can receive data from different sources
- **No storage**
- Data has to be **processed straight away** with eg. **Lambda** function and stored in **S3** or **Elasticsearch cluster**
- It’s easiest way to load streaming data into data stores and analytical tools

### **Analysis**

- You can use it to analyze your data inside Kinesis Streams or Kinesis Firehose

### Other

- Autoscalling can be used to scale Kinesis Strems – clowatch can trigger api gateway to call lambda which can change Kinesis Data Stream Shards nuber 
- YOu can also use Kinesis Scaling Utilities – automated withj AWS Beanstalk

## AWS Glue

- ETL jobs service, works fine with Athena

---

# Caching

## Cloudfront

- **CDN**
- **Distribution** – collection of edge location
- **Origin \-** resource serving the content \(eg. S3 bucket\)
- Has two modes
    - **Web distribution mode**
    - **RTMP** – for media streaming
- **S3 has its own signed urls and cookies**

### Signed urls & cookies

- Gives you possiblity to **restrict access only for specific users**
- **Signed url = 1 file**
- Can be used for **uploading files**
    **Cookie = multiple files**
- You have to define policy
    - **URL expiration**
    - **IP ranges**
    - **Trusted signers** \(which aws account can create urls\)
- **Flow**
    - **App is handling authorization**
    - **When user is authorized its requesting signed url by using AWS SDK and returning it to the client**
    - **User can request a resource directly from cloudfront by using signed url**
- Has limited lifetime

## EliastiCache

- **Cache layer for databases** boosting read performance
- Supports **Redis** and **Memcached**
- **Scalable Horizontally**

### Redis

- **Advanced datatypes** support
- **Ranking and sorting**
- **Pub/sub capabilities**
- **Persistence**
- **Multi AZ**
- **Backup�**�
- **Restore**

### Memcached

- **Multithread performance**

---

# Data analytics

## Athena

**I**nteractive query service which allows you **query data with SQL**

- Serverless
- **Pay per query / TB scanned**
- **No ETL processes**
- **Working with s3**
- Use cases
    - Querying logs
    - Queries on click\-stream data
    - Business reports for data stored on S3
- Data formats support
    - **Apache Parquet**
    - **Apache ORC**
    - **JSON**

## Macie

**ML service discovering PII data on S3 and protecting it**

- PII –Personally Identifiable information \(dane osobowe\)
- Dashboards and alerts
- Analyze CloudTrail logs
- Querying S3 data

## Redshift

AWS data warehouse \(solution for OLAP\)

- **Enhanced routing** – VPC resources get access to redshift
- Data warehousing
- Business analytics
- Petabyte scale

### Modes

- **Single node \(160 Gb\)**
- **Multinode**
    - **Leader node – manages client collections and receives queries\)**
    - **Compute node –stores data and performs queries \(max to 128 nodes\)**

### **Advanced compression**

- **compressing columns** rather than rows
- **Sampling data in column** and chooses **best compression algo**

### **Massive Parallel Processing \(MPP\)**

- **Automatically distributes data and query loads between nodes**

### Backups

- 1 day retention period by default \(up to 35 days\)
- **Three copies – original, replica** on **compute node** and **S3**
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

## EMR \(Elastic Map Reduce\)

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
    - **Master – check health of the cluster and status of the task**
    - **Task \(optional\) – only run task not storing data**
    - **Core – run tasks and stores data in HDFS \(Hadoop file system\), at least one**
- Every node communicates with each other
- Logs from master log can be uploaded to S3 in 5 minute intervals \(needs to be configured on cluster creation\) 

## AWS Batch

- **Service for running batch jobs**
- **Multi\-node parallel jobs**
- **Easily schedule jobs and launch instances according to your needs�**�
- **Needs Cloudwatch log driver installed on the machine**

---

# Security

## KMS

- Regional service
- **Manages customer master keys \(CMKs\)**
- Can encrypt S3, databases or System Manager Parameter store secrets
- **Payment per API call**
    Can encrypt and decrypt data up to 4KB in size
- Audit capability using CloudTrail 
- FIPS 140\-2 Level 2  compliant \- private keys are stored in the way that any acces would have to be noticed \(tampering\)

### KMS key

- KMS Key is a cryptographic master key
- It can be **symmetric** or **asymmetric**
- It can be also **HMAC**
- Keys are not being used outside KMS unencrypted, so are always used via KMS service
- Keys can be created by **Customer** \(Cusomer Managed Key\) or by AWS \(AWS managed key\)
- by default keys are generated by AWS but can be also uploaded \(to Cloud HSM cluster\)

### Customer managed keys

- You can manage **cryptographic material, key policies, IAM policies, grants**
- CMKs types
    - **AWS Managed CMK** \- Free, used by default, only single service can use them
    - **AWS Owned CMK** – used on shared basis on multiple account\- cannot be viewed
    - **Customer managed key –** Allow key rotation, controll via key policies and can be enabled/disabled
- **CMKs symmetry**
    - **Symmetric** \- same key used for encryption and decryption
        - AES\-256
        - Never leaves AWS unencrypted
        - Must call the KMS APIs to use
        - AWS services integrated with KMS use symmetric CMKs
        - Can be used to import your own key material
    - **Asymmetric** – public key for encyryption, private key for decryption
        - RSA and ECC cryptography
        - Private key never leaves AWS unencrypted
        - Must call KMS APIs to use private key
        - You can download public key and use outside
        - AWS services integrated with KMS do not use symmetric CMKs
        - Useful for signing messages 

### Key symetry

- By default all services which are integrated with KMS are using symmetric encrytpion
- You can create asymetric keys but in order o use them you need to use AWS KMS API to use the private key

### HMAC

- type of symmetric key of varying length that is used to generate and verify hash\-based message authentications codes

### Encryption context

- Key value pairs metadata added to encryption request
- This context is bounded to the encryption
- Request to decrypt the data has to include the same context

### Key policy

- Determines who can use KMS keys \(can be done only for Customer managed keys\)

### Grant

- policy instruments which allows AWS principals to use AWS KMS keys

### Auditing

- ### usage of the key can be checked via Cloudtrail

### ReEncryption

- it's an API operation which lets you change the key for the data
- data are being decrypted with old key and the encrypted with new one

## CloudHSM

- You can store your own keys there with **FIPS 140\-2 Level 3** security compliance 
- **Runs within VPC** in your account
    **Hardware security module**
- Single tenant, **dedicated hardware,** **multi\-AZ cluster**
- Industry standard API
- Compliant with
    - PKCS\#11
    - Java Crytpography Extensions
    - Microsoft CryptoNG
- Architecture
    - Cluster needs to be in its own **new VPC�**�
    - In client **VPC ENI** for connectivity to **HSM** are being configured
    - Not HA by default
- Backups
    - To the S3 bucket in the same region
    - HSM Generate Ephermal Backup Key \(AES 256 bit\)which is later encrypted with Persisten Backup Key \(also AES 256 bit\)

## Secret Manager

- **Charged** per secret stored and **per 10 000 API** calls
- **Automatically rotate secrets**
- **Automatic update of secrets in RDS**
- **Generate random secrets**

## AWS Shield

- **DDoS protection**
- **Shield standard**
    - **Automatically enabled for all customers at no costs**
    - Protects agains L3 and L4 attacks
        - SYN/UDP floods
        - Reflection attack 
    - Stopped 2.3 Tbps DDoS attack for three days straight
- **Shield advanced**
    - 3000$ per month
    - **Enchanced protection** for **EC2, ELB, Cloudfront, Global Accelerator, Route53**
    - **Business and Enterprise support** – **DDos response team** – **24x7**
    - DDoS cost protection 

## Trusted Advisor

- Posture management tool ?
- **online too that proides real time guidance to help you provision your resources following betst practices**
- **Provides info about real time service limit checks**

## Inspector

- Scans EC2s and container images
- Can be used across many AWS Organizations
- It emits event into the EventBridge
- It uses NVD \(National Vulnerability Database\)

## AWS Security HUB

- Posture management tool which is combining findings from:
    - AWS GuardDuty
    - AWS Inspector
    - AWS Systems manager
    - Macie
    - IAM Access Analyzer
    - Firewall manager
    - AWS Config

---

# Migration to cloud

## Snowball

- Device allowing to deliver data to AWS and retrieve it in big amounts
- **Tamper\-resistant**
- **256bit encryption**
- **Software erasure** after **successful transfer**
- **TPM \(Trusted Platform Module\)** compliant
- **Standard snowball�**� \- 50TB, 80TB
- **Snowball edge**
    - 100TB
    - Also gives you some compute capabilities
- **Snowball mobile**
    - Truck which allows you to transfer petabytes to the cloud

## Storage gateway

- **Let’s you connect your data center with AWS storage solutions**
- It’s syncing data from your data center with the cloud
- It can be **VM image** compatible with **MS Hyper\-V and VMWare ESXi**
- You can define volumes used for syncing on the VM

### Types

- **Volume gateway ISCSI** \(syncing with EBS\)
    - **Stored volumes** – you always have full copy of the data
    - **Cached volumes** – data only kept on the volume temporarily before upload to the cloud or caching data during download
- **File gateway \- NFS&SMB** \(syncing with s3\)
- **VTL – Tape Gateway** – let's you leverage your tape backup infrastructure to store backup data to the cloud \(instead of real tapes\)
- \(not working with EFS\)

---

# IaaC

## SAM

- **CloudFormation** extension for serverless applications
- New **types**: **functions**, **APIs**, **tables�**�
- You can **run** your app **locally** using **docker**
- **Package** and deploy using **CodeDeploy**

## Cloudformation

- IaaC tool for AWS
- You can create nested stacks 
- You can create Multi\-regional stack
- You can Parametrize stacks
- You can set stack outputs which can be used in other stacks
- You can import \(some\) existing resource into stack

---

# Other

## AWS Parallel Cluster

- **Cluster management tool**
- **Automation of creation of VPCs subnets and the cluster**
- WAF

## Simple workflow service

## Elastic Transcoder

- **Converting media to different formats** for different devices
- **You pay for time of usage and resolution**

## Parameter store

- Component of **AWS systems Manager** \(\)
- Provides secure **storage for secrets**
- Values can be stored in plain text or encrypted with **KMS**
- Parameters can be **stored in hierarchies** \(up to 15 levels\)
    - **Permissions** can be given **for** specific **paths**
- **Versioning�**�
- TTL can be configured for **rotation**
- Parameters can be **used directly in CloudFormation�**�
- You can use AWS SDK to **retrieve data** from **SSM parameter** store in **application code�**�
- No additional costs till quota reached 
- No secrets rotated

## Alexa

- **Skill service**
- **Skill interface**
    - Invocation Name
    - Intent Schema
    - Slot Type 
    - Utterances
- **AWS Polly** – Text to speach service 

## OpsWork

- OpsWorks configuration service using Chef and Puppet

## AWS Config

- Provides detailed view of how Cloud resources are currently configure and how were in the past
