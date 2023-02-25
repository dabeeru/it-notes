---
title: Gitlab flow
creation_date: 2023-01-22 23:03
tags:
	- DevOps
	- CICD
	- SCM
	- IT
---

# Design 

- master
- feature
    - short lived 
    - merged back to master
- production
    - contains code deployed to production
    - changes can be promoted from the master branch whenever the PROD release should occur
- other environment related branches
    - staging
    - pre\-prod