---
tags:
  - toBeRefined
  - outOfStructure
---

[[Web]]

---
# Security

# Tools
- [offsec.tools](https://offsec.tools)
# Resources
- [David Bobal's interview with STOK](https://youtu.be/-2eVJRBNzY0?t=2094) - a lot of resources recomendations
- [Fredriks podcast](https://www.youtube.com/watch?v=v7FMPU3J3Qw) - also some good stuff here
- Web application haker handbook
- Rozmowa kontrolowana - tester aplikacji weboych z googla, tez poleca kozak rzeczy

# General concepts
- Passive attack \- accessing network in order to spy and steal data\)
- Active \- attack modifying/encrypting/deleting data defacing website, bringing down service
- Types of attacks
    - Software vulnarabilities and backdoors
    - Man in the middle attack \(MITM\) \- eavesdropping the communication between client and server in order to steal data or credentials, often done by attacking wireless devices
    - Malware
    - DDoS \(often SYN flood attack\)
    - Internal Threats / Rogue Employees
- Intrusion Prevention System \- software which analyzes system telemetry in order to recognize intrusion and based on that is automatically implementing counter measures \(eg. ban some specifc IP for some time\) 
- CIA 
    - Confidentiality \- Sensitive information must be available only to set of predefined individuals
    - Integrity \- information should not be altered
    - Availability \- information should be always available
- Zero trust
    - Do not trust any device or service by default
    - Always verify its identity and compliance
    - Enforce 2FA
    - Ensure compliance of the devices
    - Use certificates for all applications
- CDR \- Content Disarm Reconsctruction \- software which is removing executable code from documents that shoudn't contain them eg. scripts from pdf documents 
- NAC \- Network access control \- software agent ensuring the compliance of the device that is meant to connect to the network
- Web filter proxy \- proxy server filtering untrusted HTTP traffic 

# General security rules

1. Keep software up to date
2. Zero trust
    1. 2FA
    2. NAC \(ensure device complaince\)
    3. Least privilidge RBAC 
3. Network segmentation
4. Proper firewall configuration
5. Centralized logging, baselining and anomaly detection 
6. Use strong passwords
7. Mutual TLS for ensuring client identity 
8. Strong encryption
9. Use VPN

## Common exploits

- Null or default passwords
- IP Spoofing \- sending network package on behalf of another host, often use to invoke kind of DoS attack
- Eacesdropping
- Service Bulnerabilities 
- DoS
- Service and Application Vulnerabilities
- CSRF
- XSS

# Securing OS

## Rules

1. Keep software and kernel up to date
2. Implement host\-based firewall
3. Disable not used services
4. Delete unwanted packages
5. [Use TCP Wrappers](https://www.tecmint.com/secure-linux-tcp-wrappers-hosts-allow-deny-restrict-access/)
6. Constant Network Monitoring
7. Install antimalware software
8. Use HIDS software \(eg. SNORT\)
9. Use IPS software \(eg. fail2ban\)
10. Implement Network Segmentation \(eg. VLANs and subnets\)
11. Use proper encryption
    - Use software which supports and configure proper encryption
    - Use proper encryption for wireless networks
12. Consider service separation
    - at least services critical form security perspective can bee kept on the separate server than other non critical services
13. Use SElinux
14. Enforce strong password policy
15. Consider 2FA
16. Secure SSH connection
17. Aggregate logs and perform security analysis
18. Enable system auditing
19. Encrypt filesystem
20. Restrict physical access to the server
    - Room security
    - BIOS password
    - Disabling boot from external devices
21. Backup the server
22. If there are some technical users disable shell for them
23. Store logs in centralised place

## Tooling

- Firewall
    - [firewalld](https://www.tecmint.com/configure-firewalld-in-centos-7/) \(RHEL\)
    - [UFW](https://www.tecmint.com/setup-ufw-firewall-on-ubuntu-and-debian/) \(debian\)
    - port scanning
        - nmap 
        - ss 
        - nmap
    - tcp wrappers 
        - firewall just cut off comunication, so attempt ot use port is not even logged 
        - tcp wrappers works on top of firewall rules 
- IDS \(Intrusion Detection System\) 
    - SNORT
    - aide
- IPS \(Intrusion Prevention Sytem\)
    - fail2ban
- Network monitoring
    - Wireshark
- Antimalware 
    - ClamAV
    - chkrootkit \- \(rootkit detection\)
    - rkhunter \(rootkit detection\) 
- Log analysis
    - logcheck/logwatch
- System auditing
    - auditd

## Encryption

Whenever possible use software that allows encryption and use stron encryption cyphers.

1. Use scp/ssh/rsync/sftp for file transfer
2. You can [use GnuPG for encrypting and signing data](https://yanhan.github.io/posts/2017-09-27-how-to-use-gpg-to-encrypt-stuff/)
3. Use VPN
4. Use SSL for web servers

Do not use tools which doesn't support proper encryption, for example:

- FTP
- telnet
- rsh

## Authentication

Proper password policy:

- at least 8 characters long
- mix of letters of different case, numbers, special characters
- enforce password rotation
- restrict using previous passwords
- disallow empty passwords

Other:

- lock user accounts after multiple login failures

### Firewall

- Can be managed with tolls like firewalld, iptables or UFW \(ubuntu\)
- Firewalld zones
    - Drop \- drop connection without answer
    - Block \- answer with icmp\-host\-prohibited
    - Public \- accept selected connections 
    - External \- zone will act as route with masquerading 
    - DMZ \- exposing service to public, only incoming connections allowed
    - home \- trust other computers in the network
    - Trusted zone \- all traffica aacctepted
    - Internal zone \- only traffic from internal network is allowed

### TCP wrappers

- Are evaluated after firewall rules
- can be configured in /etc/hosts.allow and /etc/hosts.deny
- allow precedes deny
- With TCP wrapper you can deny or allow connections to specific service from specific IP eg. allows connection to sshd only from specific IP
- Service needs to support tcp wrappers

## SELinux

- SELinux is implementation of MAC \(Mandatory Access Control\)
- It works on top of DAC \(Discretionary access control mechanism\) which is responsible for basic linux permissions
- **SecurityContext**
    - mechanism for classifying resources like files and processes
    - uses labels like _user, role, type, level_ \(usually written as _User:Role:TypeOrDomain:Level\)_
- Subject \- active process with security context assigned
- Object \- files, sockets, pipes or network interfaces accessed by processes
- Domain  \- part of security context of a subject \(eg. _User:Role:Domain:Level\)_
- Type \- part of security context of an object \(eg. _User:Role:Domain:Level\)_
- Policy \- specifies  rules on security contexsts \- which domains can access which types
- Everything is denied as default
- SELinux modes
    - enforcing \- SELinux policy is enforced, SELinux denies access based on SELinux policy rules
    - premissive \- SELinux doesn't denies anything but all denials are logged
    - disabled \- SELinux is disabled
- SELinux type enforcement
    - eg. _allow innd\_t usr\_t:file { getattr read ioctl }_ \- allow processes running in the innd\_t domain to do getattr, read and ioctl on files of type usr\_t.

## Kernel capabilities

Are allowing limit processes run as root to do only specific things

## sudo hardening

sudo allows to delegate authority to run some commands as other user \(usually root\)

- root access should be disabled, all commands should be done via sudo
- do not allow all of the commands, only those which are needed
- do not allow to become root \(user shouldn't be allowed to run su\)
- do not allow to run interactive shells with sudo \- this will open new shell with sudo permissions
- configure timeout for sudo
- log all commands run with sudo
- allow to run only commands from specific paths
- ...

## Backups

- [Fileystems can be backed up with rsnapshot](https://www.cyberciti.biz/faq/linux-rsnapshot-backup-howto/)
    - Files can be restored with rsync
    - It utilizes hardlinks so if file hasn't been changed it's just hardlinked to the previously backed up file

## Other

- No accounts beyond Root should have UID 0
- Secure BIOS
    - Set BIOS password
    - Disable booting from external devices
- Encrypt filesystem
- Disable X11 if not needed
- Use Centralized Authentication Service \(eg. over LDAP, oAuth or Kerberos\)
- Consider configuring multiple partitions with additional configuration
    - noexec \- disallow execution of any binaries on this partition
    - nodev \- do not allow character or special devices on this partition
    - nosuid \- disallow \(SUID/SGID\) \(?\)
- Disallows SUID and SGID Binaries
    - Those special permissions allow any user to run binary with permissions of owner \(suid\) or group to which binary belongs \(guid\)
- Consider disabling USB, Thunderbolt devices

# Securing SSH connection

1. Disable root login
2. Disable password authentication
    1. Use certificates
    2. Use 2FA with PAM modules
    3. If disabling password not possible
        1. Enforce password rotation
        2. Enforce stong password policy
            1. Strong complexity
            2. Not using previous passowords
        3. Disable empty password 
3. Configure idle timeout
4. Allow only specific users
5. Consider changing the port
6. Record session with script
    1. Disable X11 forwarding
    2. Disable TCP forwarding
7. Enforce strong ciphers
    1. CHACHA20 and curve25519 should be preffered
    2. Deactivate Diffie\-Hellman short moduli
8. Allow connections only from specifics networks
9. Enforce listening only on specific network interface
10. Advanced
    1. port knocking \- client has to perform a specifc order of connections to closed ports and only then its connections is allowed

#  Bastion host

![image.png](image/image.png)

- Bastion host needs to be in public subnet
- Other instances needs to be in private network
- Security group for linux instances allow SSH connection from security group of the bastion host 
- Eventurally bastion host can be in separate VPC but then VPC peering needs to be configured
- You can also configure OpenSSH to use [script](evernote:///view/201141896/s577/9b075047-e114-9edd-6e42-e6a9ad97e81a/a0f15f21-8398-8d27-7a05-cf14533b3433/) to record the interactive session
    - You need to disable X11 forwarding and TCP forwarding so it cannot by bypassed
    - And also couple other tricks need to be applied to disallow to remove session recording
- You can set up automation where bastion host will periodically download public keys of allowed users from the bucket and automatically create user account if not existing
- Users should has limited permission \(least privilage approach\) 
- All general hardening rules for linux server apply

# IDS

- IDS \- Intrusion Detections System \- software which monitors host/network, recognize anomalous activity and raises the alarm towards administrator. It can also take some action  eg. block traffic.
- HIDS \- Host based Intrusion Detection system \- Monitors system events, can backup configs and restore them to mitigate malware activity. Detects mostly things like config changes and root access. Usually distributed \- agents on hosts and centralized module.
- NIDS \- Catures network traffic selectively and analyzes it. Can also block traffic classified as malicious. Usually deployed on dedicated hardware. 

Modes of operation:

- Signature\-based IDS \- for NIDS many malicious tools always generate traffic with the same well known signature, so it can be used to quickly recognize it. Faster than Anomaly\-based.
- Anomaly\-based IDS \- looks for unusual patterns of activity, may use some AI.

IPS \- Intrusion Prevention system, Intrusion Detections systems which are considered to be active \(eg. restoring config changed by malicious activity, dropping malicious traffic, locking out malicious users.

Tools:

- Host based \(HIDS\)
    - SolarWinds Security Event manager
    - CrowdStrike Falcon \(Cloud\)
    - OSSEC
    - AIDE 
    - Fail2Ban
- Network based NIDS
    - Snort
    - Zeek
    - Suricata

IDS systems can be also used as agent generating alerts for SIEM sustem

# SIEM

SIEM \- Security Information and Event Management \- tool which allows to monitor security related information for whole network in real time. SIEM tools combines multiple security related class of tools:

- Log Management \(LMS\)
- Security Informatiom Management \(SIM\) \- aggregating and managing information about, firewalls, DNS servers, Routers and antiviruses
- Security Event Management \(SEM\)\- proactive monitoring and analysis, correlating and visualising data
- User Behaviour Analysis \(UBA\) \- usually AI based solution that is analysing software

SIEM might be a main tool in arsenal of a blue team, is used to automatically detect security issue raise the alarm and potentially mitigate it automatically.

Rules of using SIEM:

1. Design and apply predefined data correlation rules across all system
2. Identify business compliance requirements and ensure that SIEM is enforcing them
3. Reduce false positive on daily basis
4. Prepare incident response plans
5. Have clear incident event prioritetization rules to handle incident response properly

SIEM tools:

- Datadog Security Monitoring
- SolarWinds Security Event Manager
- LogPoint
- Graylog
- Splunk enterprise Security
- IBM QRadar

# Attack patterns

## Cross\-site request forgery \(CSRF\)

Malicious code in the browser can trigger a specific action on vulnerable website \(eg. email change\) and if only user is logged in \(so browser has a session cookie\) and using a cookie is sufficient \(there is no additional security mechanism\) action will be performed.

Can be also partly mitigated with SameSite Cookies....

## Other

**Spoofing**

- General term for attack describing situation when one program or person identified by another
- Eg. hijacking traffic for domain or ip address
- **Replay** \-  any attack including delaying or repeating traffic from antoher session 

## Replay attack 

Any type of attack which includes replying previous network communication in different session.

# Malware types

- **Virus** \- program replicating themself across the network by exploting vulnerabilitites of other software in order to corrupt or destroy the network   
- **Trojan \-** malware creating a backdoor on the infected computer
- **Spyware \-** malware gathering information about the person or organization
- **Adware \-** malicious software, enforcing some ads
- **Ransomware \-** malware encrypting software for money

# [[CSPM]]

# AWS Security

[https://aws.amazon.com/architecture/security\-identity\-compliance/?cards\-all.sort\-by=item.additionalFields.sortDate&cards\-all.sort\-order=desc&awsf.content\-type=\*all&awsf.methodology=\*all](https://aws.amazon.com/architecture/security-identity-compliance/?cards-all.sort-by=item.additionalFields.sortDate&cards-all.sort-order=desc&awsf.content-type=*all&awsf.methodology=*all)
