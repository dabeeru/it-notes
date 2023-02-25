---
title: GitHub flow
creation_date: 2023-01-22 23:02
tags:
	- IT
	- SCM
	- DevOps
	- CICD
---

# Branches
- master
- feature 
    - branched out from master
    - short\-living
    - merged back to master

# **Pros**

- Aimed for having often merges to master and short feedback loops
- Much simpler than GitFlow

# **Cons**

- Not suitable for maintaining multiple versions of production code 
- No branch \(like develop\) which can be used for testing changes before merging to master \(this verification needs to happen on the feature branch\)
- Much more prone to merge conflicts in case of having multiple teams working on the same shared codebase
