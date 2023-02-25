---
title: Trunk based
creation_date: 2023-01-22 
tags:
  - SCM
  - IT
  - DevOps
  - CICD
---
# Design

- No branches only master
- Changes related to the features should be merged to master at least daily
- Feature toggles should be used in order to disable the features which are not yet ready to be used on production

Feature flags..

# **Pros**

- Eliminates any merge conflics
- Forces often integrations
- Really suitable for single developer projects

# Cons

- May lead to unstable version of code on master
- Requires bigger majority of the developers since every feature should be always in ready to release state

