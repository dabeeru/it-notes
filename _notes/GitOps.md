# GitOps
- One repository with declarative definitions of the environment
- Deployent should be fully automatic
- Git is audit log for the environment

## Architecture
- GitOps doesn't provide the way to promote the changes from stage to another

### Repositories
#### Application repository
 - Application source code 
 - Deployment definition eg. templates for K8S yaml's
 - *What about configuration (eg. ENV variables)*

#### Environment repository
- Infrastructure only 

### Deployments
- Multiple environments = multiple branches of the repository

#### Push-based
![[Pasted image 20230227201808.png]]
- Application pipeline is building the image and updating Environment repository with image id and deployment templates
- Change in environment repository triggers deployemnet pipeline

#### Disadvantages
- Pipeline has to have broad permissions to setup the environment
- It won't detect devations of desired state if something would be changed manually on environment

#### Pull-based
![[Pasted image 20230227203242.png]]
- Operator is constantly observing the infrastructure and environment repository
- Operator is applying all the changes made to repository and reverting all of the changes made directly to the environment
- Operator's health should be monitored
- If Operator fails to bring environment to the desired state some alerts should be triggered
- Operator should leave within the same environment


### Microservices
![[Pasted image 20230227204427.png]]
