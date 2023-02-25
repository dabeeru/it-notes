
- Secure private connectivity for services between separate VPCs
- VPC Endpoint \-\> VPC Service Endpoint \-\> NLB \-\> Service within VPC \( can be used within one region\)
- Lets you **to share services in one VPC with multiple different VPCs**
- **Easier to do that** on big scale than by **VPC peering**
- **More secure than opening VPC** to internet \(then you have to harden it\)
- Requires **NLB on service VPC** and **ENI on the client VPC**
