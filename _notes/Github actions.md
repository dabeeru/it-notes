---
title: Github actions
creation_date: 2023-02-01
tags:
	- CICD
	- DevOps
	- IT
	- CatalyX
---


# Components
![[Pasted image 20230228133929.png]]

## Events
- Pull request
- Issue creation
- Release creation
- Pushing a commit

## Workflow
- Worflow is an automated process which is running one or more jobs
- Workflows are defined by YAML files kept in `.github/workflows` directory
- Workflow can be triggered based on the events

## Job
- Jobs can be executed in sequences or in parallel
- Jobs can be run on the *runner* machine or in the container
- Jobs can have multiple steps
- All Steps included in Job are executed on the same runner

## Step
- Steps can be defined by scritps or *actions*  
- You can sahre data beetween the steps
- If jobs do not have dependencies to each other they are run in paralell

## Actions
- Actions are just plugins that allow to perform repetable actions 

## Runners
- Runners are by principle virtual machines responsible for running jobs
- Every job execution uses newly provisioned virtual machine
- They can be also containers

