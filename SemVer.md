---
title: SemVer
creation_date: 2023-01-21 11:53
tags:
	- IT
	- DevOps
	- CICD
---
- Semantic versioning is a convention for versioning software API, indicating what changed exactly in every release.
- It's clear what changed in the software and if change is backward compatible or not based on which identifier within the version changed .

**Structure**

- X.Y.Z\-pre\-release.identifires\+buildnumber 
    - X \- Major \(Backward incompatible changes\) 
    - Y \- Backward compatible features
    - Z \- Backward compatible bug fixe
    - pre\-release \- identifier indicating that it's pre release version which might not be stable
    - buildnumber \-  CI build number
- Major version can contain also backward compatible changes and fixes
- Minor version can conatin also bugfixes
- Pre\-release version always have lower precedence before other versions
- Build number is ignored when determining precendence
- Version 0.Y.Z is considered as not stable \- api can be change by incrementing Minor identifier
- When API is mature enough to provide backward compability it should be versioned as 1.0.0 for first time
- If there is backward incopatible change released within minor or patch release it should be corrected with next minor release and then \(optionally\) released properly as Major release
- If you are deprecating functionality within API you should announce it with Minor release and only after that remove it with Major release

### What is backward incompatible change?

It's everything that forces client to change their code, examples: 

- Changing endpoint path
- Changing request payload by adding new required fields
- Changing response payload by removing some fields or changing their type
    - Even adding new fields may be considered as breaking since client my for example fail on data serialization
- Chaning function contract
