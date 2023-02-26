---
title: EKS
creation_date: 2023-02-01
tags:
	- AWS
	- Cloud
	- IT
	- Compute
	- Kubernetes
	- CatalyX
---
# EKS
## Control plane
- HA ibn multiple AZs
	 - at least two API server instances and three etcd
- Autoscalling 
- Fail tolerant - replacing unhealthy nodes
- Auto update
- Single tenant (not shared with other cluster or AWS accounts)

##  Nodes
- Can be self-managed or AWS-managed
- Can be deployed on Fargate

## EKS deployment options
- EKS - standard option
- EKS on [[AWS Outposts]] - allows to use on-premise servers, only nodes or also control plane
- EKS Anywhere - allows you to  install [[EKS Distro]] in your local datecenter (provides AWS support)
- [[EKS Distro]] - you can install EKS Distro on your own (no AWS support)

## eksctl
- Command line tool for managing EKS cluster

## Cluster setup
### Prerequisites
- VPC with subnets in multiple AZs

## Updates
- You can trigger update of the control plane
- Rollout of the update includes healthcheks and rollback in case of issues
- Requires free adresses in subnets

## High availability
- Multiple autoscalled node groups in multiple AZs
- Node groups can be ballanced
- Every stateful application should have data sharded across AZs - EBS volumes can be attached to the nodes within single AZs 

## Autoscalling

### Cluster Autoscaler
- Based on Autoscalling group
- Require specifc Deployment configured on the cluster
- Deployment should be deploy in HA (supports leader election)
- Spins up new nods if:
	- Some pods has been terminated becouse of insufficient resources
	- Some Pods are underutilized (CPU starving)
- Support scaling out to 1k of nodes
- There are some options for vertical scalling (adding more resources to nodes)
	- When scalled vertically when hypervisor is provisioning more resources then all of the data associated in pods is kept in memory 
 
#### Overpovisiong
- You can enable overprovisiong to keep some resources buffer and ensure that there will be always some free resources that can be utilized without waiting for creation of new node
- AVG node request frequency / Time to provision new node = Number of nodes to overprovision

#### Considerations
- Preferebly configure mutliple nodegroups in multiple AZs
- You can label nodegroups and enable balancing 
- Smaller number of nodes but stronger machines is usually easier to scale

### Karpenter
 - Open source alternative to Cluster autoscaler

### Vertical Pod Autoscaler
- Automatically adjusts pod requirements according to usage

## Cluster management
- Dashboard can be deployed
- kubecosts can be used for costs monitoring (requires prometheus)

## Observability
- Control plane logs are exported to cloudwatch including:
	- api server
	- audit
	- authenticator
	- controller manager
	- scheduler
- AWS Distro for Open Telemetry is available

## Security
- When cluster is created IAM Principal used for creation is added to K8S RBAC authorization table as the administrator

### IAM  integration
- You can map IAM Role to the K8S Service account, and credentials will be automatically injected to the container (AWS Credential chain applies)

### Authentication
- IAM
- OIDC

### Secret encryption
- Can be done with KMS

### Cluster endpoint
- By default only public endpoints is created 
- Access to the endpoints is based on AWS Identity and IAM

#### Public endpoint
- Public access can be restricted or limited to specific IPs
- Wheen used nodes also communicate with Control plane via public endpoint (traffic is leaving VPC but not AWS network)
- can be disabled
- DNS name is assigned by managed [[Route53]] hosted zone

#### Private endpoint
- Traffic to control plane is routed only through VPC

## Networking
- [Demystifying cluster networking for EKS](https://aws.amazon.com/blogs/containers/de-mystifying-cluster-networking-for-amazon-eks-worker-nodes/)
- NACL and Security groups must allow for communication to control plane
- VPC confiuration for nodes must have `enableDnsHostnames`, `enableDnsSupport`, `AmazonProvidedDNS`


## Other
### Windows support
- Still at least one Linux node must be present (for components like CoreDNS)
- There is quite a lot of litmitations
