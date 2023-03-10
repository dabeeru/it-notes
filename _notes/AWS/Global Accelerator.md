- **GA directs traffic to optimal endpoints over the AWS global network**
- User is getting through **Edge locations** through AWS Global Accelerator which **based on proximity, health and endpoint weight** choosing **optimal endpoint group,** from which traffic is routed to the true endpoint \(ALB, NLB, EC2\)
- By default, you are getting **2 static IP public addresses**
- **It lowers the cost of data traffic**
- It directs connection to the correct region
- Allows quick failover to another region

### Architecture

- **Static IP address**  by default [1.2.3.4](http://1.2.3.4) and [5.6.7.8](http://5.6.7.8)
- **Accelerator**
- **Network ZONE**  like AZ, gives you two private static addresses
- **DNS Name**
- **Listener**
    - inbounds connections from clients to Global Accelerator, based on port \(or port range\) and protocol that you configure, both TCP and UDP are supported
    - Each listener has one or more endpoint groups associated with it and traffic is forwarder to endpoint in one of the groups
    - **Client affinity** you can set up listener for specific ip address or range (?)
- Endpoint Group
    - Each **endpoint group** is associated with a **specific AWS Region**
    - Endpoint groups include one or more endpoints in the region
    - Traffic dial  you can reduce or interface the percentage of traffic that would be otherwise directed to an endpoint group
        - Useful for performance testing or blue / green deployment
- Endpoint
    - **ALB, NLB, EC2, Elastic IP address**
    - **Endpoint can have their weights**
    - ALB can be internet facing or internal
    - **VPC Endpoints**
- Endpoints are virtual devices
- Endpoints are horizontally scaled
- Between regions VPC needs to be peered in order to VPC endpoint to work
- **Types**
    - **Interface endpoint**  **ENI** with **private IP address**, serves as an entry point for traffic destined to a supported service
    - **Gateway endpoint**  like **NAT gateway,** supported only for **DynamoDB** and **S3**
