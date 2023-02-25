---
title: Cloud Concepts
creation_date: 2023-01-22
tags:
	- ToBeRefined
	- ToBeDiscovered
	- IT
	- Cloud
---
## Landing zone
* [https://aws.amazon.com/solutions/implementations/aws-landing-zone/](https://aws.amazon.com/solutions/implementations/aws-landing-zone/)
* [https://docs.aws.amazon.com/prescriptive-guidance/latest/migration-aws-environment/understanding-landing-zones.html](https://docs.aws.amazon.com/prescriptive-guidance/latest/migration-aws-environment/understanding-landing-zones.html)
* [https://docs.aws.amazon.com/prescriptive-guidance/latest/migration-aws-environment/building-landing-zones.html](https://docs.aws.amazon.com/prescriptive-guidance/latest/migration-aws-environment/building-landing-zones.html)
* [https://successive.cloud/cloud-landing-zone-why-organization-need-it/](https://successive.cloud/cloud-landing-zone-why-organization-need-it/)
* [https://www.meshcloud.io/2020/06/08/cloud-landing-zone-lifecycle-explained/](https://www.meshcloud.io/2020/06/08/cloud-landing-zone-lifecycle-explained/)

## North-south, east-right model
It's a model describing different types of traffic in data center/cloud environment
-   North traffic - traffic going outside
-   South traffic - traffic going inside
-   East-West - internal traffic between hosts in private network
In general East-West traffic was considered as trustworthy while North and South as untrustworthy, but modern zero trust approach to security assumes that every traffic should be untrusted by default.

[https://aws.amazon.com/blogs/networking-and-content-delivery/deployment-models-for-aws-network-firewall/](https://aws.amazon.com/blogs/networking-and-content-delivery/deployment-models-for-aws-network-firewall/)

  
