

- Connecting **AWS resources** with **on\-premises Microsoft AD**
- **Standalone directory** in the cloud
- **SSO to any domain\-joined EC2 instance**
- **AD specifics**
    - **Hierarchical**  trees and forests
    - **Group policies**
    - Based on **LDAP** and **DNS**
    - **Kerberos, LDAP** and **NTL authentication**
    - **Highly available**
- **AWS directory** provides **AD domain controllers \(DCs\)** in **HA mode** in **multiple AZs**
- **DCs are not shared** with any other users of the cloud
- You **can extend this AD to on\-premise machines** with **AD Trust**
- Automated patching, Snapshot and restore
- **VPN or Direct Connect connection required**

### **AD connector**

- **Connecting cloud auth** with **local AD**
- On\-premise users can **log to the cloud with AD credentials**
- Joining EC2 instance to existing AD

### **Simple AD**

- **Basic AD features**
- **Small  500 users**
- **Large \- 5000 users**
- **Do not support AD trust**
- Works with **LDAP** for **linux machines**

### **Cloud directory**

- **AWS native directory**
- **Fully managed**
