### Mutable infrastructure
- Upgrading infrastructure is done by changing the state of current machine
- It's possible that upgrade will stuck in inconsistent and not well undestood state

### Immutable infrastructure
- Upgrading infrastructure is done by creating new VM with the desired state and decomissioning of the old instance 
- Inconsistent sate is not possible 
- Data should be extenralized to separate storage
- Optionally EBS like cloud storage can be use to attach old volume to new machine
