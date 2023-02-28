---
title: DNS
creation_date: 2023-02-12
tags:
	- IT
	- SysOps
	- Networking
---
- Domains in Global DNS system are controlled by **IANA** \(Internet Assigned Numbers Authority\)
- Domains are registered to top level domain \(.com, .[co.uk](http://co.uk) etc.\) by domain registrar through InterNIC which is a service of ICANN who is knowns as the WhoIS database
# Records
- **SOA** \(Start of Authority\)  name of the server that supplied the data for zone, administrator of the zone, current version of data file, default TTL
- **NS** \(Nameserver\)  Nameserver contains authorities DNS record, to level domain servers are redirecting traffic DNS to this server
- **A** \(Address\)
- **CNAME** \(Canonical Name\) \- aliasing one domain name with another
- Alias record \(AWS specific\) \- let you map Domain to ELB, CloudFront or S3
	- CNAME cannot be used for naked domain names
	- Route 53 allows you to create an Alias record at the top node of a DNS namespace \(zone apex\)
- **MX**  mail
- **PTR** record opposite A record