
**Everything is denied by default**

# ARN
- Patterns:
	- `arn:partition:service:region:account-id:resource`
	- `arn:partition:service:region:account-id:resource_type:resource`
	- `arn:partition:service:region:account-id:\resource:qualifier`
	- `arn:partition:service:region:account-id:resource_type:resource:qualifier`
- **Partition** usually `aws` but can be different (`aws-cn = AWS china`)
- Some parameters can be omitted
- **Wildcards** can be used
- every user also has arn - `arn:aws:iam::123456789012:user`

# Features
- [[MFA]]
- [[Identity Federation]] - [[Active Directory]], Linkedin, Facebook
- Password rotation
- [[PCI DSS]] \(Payment Card Industry Data Security Standard\) Compliant

# Structure
## **IAM Identities**
### **IAM User**
- Can be assigned withe permission directly (should be barely used)
- Can assume role
- Has password and access key
- **Access key id** and **Secret access key** are like user and password for programmatic access to the cloud
- User doesn't have any default permission after creation

### **IAM Role**
- **IAM identity** which can be be assigned with some **Policies** 
- **Role** can be **assumed** by some **AWS resource** or **IAM user**
- User can assume **only one Role** at a time 
- Assuming role results in generating temporary credentials which can be used to sign AWS API request

## Groups
- Allows grouping users
- Group can be assigned with permissions so user can inherit permissions from the group
 
##  IAM Policies
- **json documents** defining what can be done on specific resource
- Types of policies
	- **Identity policy** permission can be **attached to resource group or role**
	- **Resource policy**  **can be attached to the resource**, can define who and what actions can be made (eg. S3 bucket policy or KMS key policy)
- **Policies** do not have effect until are assigned to resource or user
- Every policy consists list of statements where each statement is related to some AWS API request
	- Allow/Deny
	- Action
	- Resource (ARN)
- There are AWS managed policies which are created in AWS out of the box
- You can define **inline policy directly in roles**

### Permission boundries
- **Permission boundaries**  limits what are maximum permissions that can be attached to user or role by identity or resource based policies
- Can be easliy used to allow specific user to use/role only specific AWS services
 ![[Pasted image 20230211193523.png]]
- [[AWS Service Control Policies]] if set up are also taken into account while setting permissions


# Root user
 - After AWS account is created , there is always AWS user assigned to it
 - This user has full access to all resources and services and should be used only to create another IAM entities and assigned them some permissions
 - Remember enable 2FA for the root user
 - Remember to rotate credentials regularly
 
# Other
- You can generate **sign in link** after creating account for first logging
- There is **root account** where the story begins
- IAM service is global
- You can utilize tags to make access to make permission management easier