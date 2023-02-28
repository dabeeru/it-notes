

- You can store your own keys there with **FIPS 140\-2 Level 3** security compliance
- **Runs within VPC** in your account
    **Hardware security module**
- Single tenant, **dedicated hardware,** **multi\-AZ cluster**
- Industry standard API
- Compliant with
    - PKCS\#11
    - Java Crytpography Extensions
    - Microsoft CryptoNG
- Architecture
    - Cluster needs to be in its own **new VPC**
    - In client **VPC ENI** for connectivity to **HSM** are being configured
    - Not HA by default
- Backups
    - To the S3 bucket in the same region
    - HSM Generate Ephermal Backup Key \(AES 256 bit\)which is later encrypted with Persisten Backup Key \(also AES 256 bit\)
