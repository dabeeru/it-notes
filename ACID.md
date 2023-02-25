---
title: ACID
creation_date: 2023-02-02
tags:
	- IT
	- Data
	- CatalyX
---
### **A**tomicity
Each tranaction is treated a single unit - if any of operations in transaction fails, whole transaction fails.

### **C**onsistency
Transaction can only move database from one consistent state to another consistent state.
Consistency needs to be guaranteed regarding all constraints triggers etc.

### **I**solation
Even if there are multiple transaction executed in parallel then all transactions should end with database in state as if they would be run in sequence.

### **D**urability
Once transaction is commited it will remain commited even in casse of a system failure. Thi mean that completed transactions are recorded in non-volatile memory.
