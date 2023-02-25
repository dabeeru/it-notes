---
title: AWS Organizations
creation_date: 2023-02-11
tags:
	- IT
	- Landing zone
	- Cloud 
	- AWS
---
- Organizations let you group **multiple AWS accounts**
- Organization consists **root account** and possibly **multiple Organization units**, where everyone can group **multiple AWS accounts**
- You can manage **billing globally**
- You can manage **policies globally** in **organization**
- It **reduces costs**  in most cases **more you use, less you pay, so by grouping multiple accounts** volume of services usage gets sum up
- You shouldnt set up any resources for root account
- You can use [[AWS Service Control Policies]] to **enable/disable** whole **services for specific Organization Units**
- **Accounts** can be added to organizations by sending **invitation**
- **Account** can be assigned only to one **organization**
- When you want to move account from one org to another you need to remove it from the first one and send invitation
- For moving master account to new organization, old one needs to be deleted first
