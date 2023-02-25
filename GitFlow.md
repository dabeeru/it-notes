---
title: GitFlow
creation_date: 2023-01-22 
tags:
	- DevOps
	- IT
	- SCM
	- CICD
---
# Branches
- develop
- master
- feature 
    - branched out from develop 
    - merged back to develop after feature is complete
- release 
    - branched out from develop
    - used to prepare new release version
    - merged back to develop and master
- hotfix 
    - branched out from master
    - used to fix production bug
    - merged back to develop nad master

All branches beyond master and develop are usually meant to be short\-lived.

# Pros

- Allows parallel integration of new development \(on develop branch\) while keeping production code safe \(on master branch\) 

# Cons

- Generally complicated
- Not a best idea for CI integrations
