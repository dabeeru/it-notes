---
title: AWS Cognito
creation_date: 2023-02-01
tags:
	- IT
	- Cloud
	- AWS
	- IdentityManagment
	- Catalyx
---

- **Managed directory for SaaS**
- **Sign\-up** and **sign\-in** capabilities
- **Integrates with social media** identities
- **Web identity federation**  gives user access after successful authentication with web\-based identity provider like Amazon, Facebook or Google \(user gets auth code from Web ID provider, which can be traded for temporary AWS security credentials\)
- Cognito is web federation serice in AWS
    - **Sign\-up and sign\-in**
    - **Access for quests**
    - Acts as an **Identity broker** between your **application** and **Web ID** providers
        - After auth gives you **temp credentials** which are **mapped with some IAM role**
    - User pools
        - **User Directory** managing sign in and sign up
        - Can **store the credentials** or **interface with some WEB ID provider**
        - Authentication generates JWT token
    - Identity pools
        - Provides temporary AWS credentials to AWS services like S3 or DynamoDB
    - Flow
        - **Users logins with facebook \(or something else\)** when user is **authenticated based on data in User pool**
        - **Cognito returns JWT token**
        - **User sends JWT token to identity pool** so **temporary AWS credentials** are **send to the client**
    - Cognito tracks associations between user identity and various devices used by the user
    - Cognito can be uses to **push updates and synchronize user data across multiple devices**
    - Cognito **uses SNS** to send notifications to all devices associated with a given user
    - **SAML** support
    - **OIDC** support

### **AWS SSO**
- You can **login to AWS** with **Active Directory credentials**
- You can use AWS SSO to login to Apps like GSuite or office360
- Based on **SAML**
