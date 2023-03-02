# General principles
- Stop guessing your capacity needs - just scale
- Test on scale of production - you can create environment for load test on demands
- Automating makes architectural experminetntation easier
- Don't hesitate to change - in cloud it's much easier
- Make data driven decisions
- Game days - simulate different events which may occur on production

# Operational excellence
- Use Infrastrcutre as a code
- Make frequent, small and reversible changes
- Refine operations procedures frequrently
- Anticipate failure
	- Response procdures
	- Understand impact of each possible failure
	- Test failure
- Learn from failures

## Organization
## Prepare
## Operate
## Evolve

...

# Security
- Identity foundation
	- least privilage
	- authorization
	- elimintae reliance on long-term static credentials
- Tracibility
	- monitor
	- alert
	- audit in real time
- Cover all layers
	- Edge of netwrok
	- VPC
	- Load balancer
	- Every compute service
	- Operating system
	- application
	- code
- Use IaaC for security checks
- Encrypt in transit and on rest
- Keep people away from data
- Incident managment 
	- incident response
	- investigation policy 
	- incident response simulation

## Segregation
- Segregate different workloads on separate AWS accounts

## IAM
- Enforce MFA
- Centralize IAM management 
- Use Identity Federation
- Respect Least Privilage rule
- Credentials shouldn't be shared by any user or system
- Calls to the API should be done only by using temporary credentials 

### Identities
- Humans 
- Machines

## Detection
- Monitor
	- Logs
	- Events
	- Audit trails
- Set up alerting in case of uncompliant resources creation
- [[AWS Config]] can provide you with config history
- [[AWS GuardDuty]] - helps detect malicous behaviour

## Infrastructure protection
- Configure networking tightly
- Use multiple layers of defence
- Monitor points of ingress and egress
- Use hardened AMIs

## Data protection
- Use versioning to protect from unintended overwrites and eletegions
- Use Higly available sotrage
- Have detailed logging
- Rotate keys used for encryption 
- Classify your data and apply different levels of security based on the classification

## Incident response
- Detailed login is a key
- Responses can be automated based on log processing
- You can provision *clean room* to carry out forensics in a safe isolated environment

# Reliability
- Automatically recover from failure
- Test recovery procedure - simulate failure 
- Scale horizontally for high availability
- Do not guess capacity - just scale
- Use IaaC

## Foundations
- Sufficient network bandwith to data center
- Monitor usage of service quotas

## Workload architecture
- Use Microservice architecture
- Ensure that in case of failures system capabilities degrade gradually
- Ensure that workload can easily recover from failure

## Change management
- Monitor logs and metrics
- Set up alerting
- Use autoscalling

## Failure management
- Failure shouldn't impact service availability
- Services should automatically recover from failure
- Set proper RTO (Recovery Time Objective) and RPO (Recovery Point Objective)
- Utilize Failuire boundaries - failure of one boundary wouldn't impact other boundaries
- Workloads which are meant to have low MTTR (Mean time to recover), must be architected with resiliency in mind
- Test your workload exstensively
- Design and test Disaster recovery procedure



