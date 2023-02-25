---
title: Conventional Commits
creation_date: 2023-01-21 12:00
tags:
	- SCM
	- DevOps
	- CICD
---

- Conventional commits is convention of commit messages

### Structure

type\(optional\-scope\)\!: Message

Body Paragraph

Another body paragraph

Main types:

- feat
- fix

Other types are also allowed like:

- ci
- docs
- perf
- chore
- refator
- style
- build
- test
- infra

Also _revert_ keyword can be used when doing reverts.

Breaking change can be indicated by the footer BREAKING change or \! added to type.

Scope can be anything describing specifing part of code.

### **Relation to semver**

- feat corresponds to minor
- fix corresponds to patch
- when \! or BREAKING CHANGE header is used then it corresponds to Major

### Other

- You can use some automatic tools to process the commit messages
    - Generate changelog
    - Bump versions automatically 
- If single commit contains changes for both types it should be really multiple commits.
- if errors are made  fix them with interactive rebase if not merged yet, if already merge then fuck it.