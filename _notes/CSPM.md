- Cloud Security Posture Management
- Cloudguard has [terraform provider](https://registry.terraform.io/providers/dome9/dome9/latest/docs)
- [[AWS Security HUB]] might be some alternative natively on AWS

## Rules

### Encryption in transit
- Strong cyphers
- Allowing only encrypted communication \(eg. enforcing SSL for webservers and s3 buckets etc.\) 
- Notify when certificate is going to expire

### Resource tagging
- Data Privacy
	- Confidential
	- Internal
	- Public
 
### Encryption at rest
- Enforcing encryption 
- Use of strong keys controlled by organisation \(no provider managed keys\) 
- Enforce keys rotation
- What to encrypt
	- Databases 
	- S3 buckets
	- Hard drives and other filesystems
	- Queues and datastreams
	- Backup
	- Snapshot
	- Images
	- Logs
	- Audit logs
	- Secrets
 
### Firewall configuration
- Ensure that all traffic is restricted by default \(eg. default security group restricts everything by default\)
- Ensure that traffic is not opened for ports which indicate unencrypted communication
	
### IAM - zero trust 
- IAM
	- Policies should be attached to group or roles, not specific users
- Other policies \(eg. S3 bucket policies\)
- Disable public access \(eg. s3 buckets not publicly available\) 
- Ensure that root user is not used

### K8S
- Limit connection to the API server to specific VPN network

### WAF
- Ensure WAF associations
- And proper WAF rules
	
### DDoS
- Ensure Certificates are valid
    - Alarm before expiration

### Backups
- Check if done and exported to another region

### Telemetry
- eg. VPC flow logs or EKS control plane logs 

### Leftovers resources
- Eg unused security groups or IAM roles

### Updates
- Ensure that software is updated 

### Secrets
- Ensure secrets rotation 
- Ensure user credentials rotation
    - Eg. client\_id and secret\_id for programatic access
	
### Auditing
- Ensure that audit is enabled 
- Ensure encryption
- Ensure CloudTrail logs integrity

### Authorization
- Ensure proper configuration of ID Federation \(eg. OIDC, SAML\) 
- Disable password configuration
- Ensure 2FA \(on CLoud level might be applied only to root account since rest is going through OIDC\) 
- Disable programatic access for root account
- Remove unused credentials
- Ensure MFA for root
- Ensure there is not root account access key
- Credentials not used for over 90 days should be disabled
- Trigger calls on unauthorized API calls
- Trigger alarm on root account usage

### Ensure alerting is enabled
- There might be some alarms triggered when someone is changing something which shouldn't be changed or unauthorized Cloud API calls attempts

### Compliance services
- Eg. AWS Inspector, AWS Macie, AWS Guard Duty 

### Monitoring
- Ensure that proper metrics are collected 

### Reconfigurations
- Sent alarms whenever any important config from security perspective got changed (eg. NACL configuration)

## Certificate 
- [CCSK](https://cloudsecurityalliance.org/education/ccsk/)

List some cloud guard rules

[https://www.checkpoint.com/cyber\-hub/cloud\-security/what\-is\-cspm\-cloud\-security\-posture\-management/](https://www.checkpoint.com/cyber-hub/cloud-security/what-is-cspm-cloud-security-posture-management/)

[https://www.rapid7.com/info/what\-is\-cloud\-security\-posture\-management\-cspm/](https://www.rapid7.com/info/what-is-cloud-security-posture-management-cspm/)
