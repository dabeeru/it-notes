---
tags:
  - IT 
  - SysOps
  - Containers
	- Kubernetes
---
# **Kubernetes**

## Architecture
![[Pasted image 20230217120805.png]]

### Master
- **API server** – API of kubernetes 
- **Etcd** – key\-value store \(distributed\) 
- **Kubelet** – Kubernetes Agent running on cluster
- **kube\-proxy \-** network proxy responsible for enforcing network rules
- **Container Runtime** – docker or rkt or cri\-o 
- **Controller** – observing pods and reacting when cluster state does not match desired state  
- **Scheduler** – distributing work across nodes

### Node
- **Container Runtime**
- **Kubelet**
- **kube\-proxy**

## Namespaces
### Default namespaces
-  default \- default namespace
-  kube\-system \- pods for k8s internals
-  kube\-public \- for resources that should be available for all users

### Quotas
Quotas can be assigned to namespaces.

## Workloads
- **Deployment** - for stateless services
- **Statefulset** - for stateful services
	- ensures static pod identity 
	- enables to create tempate for PVC
- **DaemonSet** - spinning up one pod per node for some background tasks
- **Job** - for some batch jobs
- **CronJob** - for some scheduled batch jobs

## Auto scalling
- **HorizontaPodAutoscalller** - scaling pod number based metics like CPU load
    - _There is also an option to scale based on custom metrics (looks like they need to be submited to K8S and the retrieved from API)_ 

## Services
### ClusterIP
- used for load balancing between pods in the group
- not available from outside of the cluster
	- at least not without Kubernetes proxy \(useful for development\)
 
### NodePort
  - exposing pods under one port on every node
  - creating the **ClusterIP under the hood** 
  - Port range \- **30,000 to 32,767**
  - Can be used to implement **external load balancing**
		
### LoadBalancer
  - only available in the Cloud
  - configuring cloud native load balancer
  - configuring NodePort and ClusterIp under the hood
	
### ExternalName
- mapping service to external DNS Name

### Headless
- **ClusterIP: None**
    - **no single IP** is being assigned to the service
    - pods has their own DNS names \<pod\_name\>\-\<pod\_number\>.\<service\_name\>
    - Useful for statefulset's

## Network policies
- Network policies allows **controlling ingress and engress traffic** based on **path** or **subdomain**
- **Selectors** are used to **control traffic** with different targets like **pods**, **namespaces** or specific **ip blocks**
- Target of network policy can be single IP address, even outside the cluster

## Ingress
- Ingress
    -  allows to specify routing rules
    - requires ingress controller \(eg. nginx, traeffik\) to be configured in the cluster
    - Ingress controller needs to be exposed to the outside via NodePort or LoadBalancer service
- When using K8S in datacenter **you need proxy server** tho share application on port 80 \- **K8S can only allocate nodeport's**
-  In **cloud** it can be done **automatically** by **service type LoadBalancer**
-  **Ingress L7 load balancer** is being set up automatically internally in the cluster \- eg. nginx

# Persistence
- PV is for adminitrators
- PVC is for developers
- PVC and PV is always mapped 1v1
- StorageClasses should classify PV by type of storage
- Always the smallest PV which is meeting requirements is being bounded to the PVC
- Reclaim policies
    - Retained
    - Recycled
    - Deleted
- storageClass: "" \- storageClass = any
- PV can be mounted to the Pod in block device mode

## Pod commands
- command parameter overrides ENTRYPOINT
- args overrides CMD

## DNS in K8S
-  DNS might be resolved differently based on K8S namespace
-  In the **same namespace** it can be just **service\-name**
-  In different **namespace** it must be **pod\-name.namespace\-name**
-  It always can be **explicitly** **pod\-name.namespace\-name.svc.cluster.local**

## Service Accounts
-  SA's can be used to interact with K8S API
- **RBAC permissions** can be applied to the service account
-  While **SA** is being created there it also **gets** corresponding secret containing **auth token**
-  Every namespace has its own service account by default
-  Such secret is mounted in every pod in namespace by default in /var/run/secrets/kubernetes.io/serviceaccount/token

## Configs

### Secret types
- **Opaque \- arbitrary user\-defined data**
- kubernetes.io/service\-account\-token \-service account token
- Docker
    -  kubernetes.io/dockercfg \- serialized ~/.dockercfg file
    -  kubernetes.io/dockerconfigjson \- serialized ~/.docker/config.json file
- Auth
    -  kubernetes.io/basic\-auth \- credentials for basic authentication
    -  kubernetes.io/ssh\-auth \- credentials for SSH authentication
    -  kubernetes.io/tls \- data for a TLS client or server
    -  bootstrap.kubernetes.io/token\- bootstrap token data

## Worloads

### ReplicaSet
- **Replication controller** \- **obsolete** controller **replaced by ReplicaSet**
-  **ReplicaSet** 
    - Controller used to create and **control multiple instances of pods of the same definition**
    - Used as **lower abstraction layer below Deployment**

### Deployment
-  Abstraction **on top of ReplicaSet** which provides **handling of rolling updates, rollback** etc.
-  Roll out **strategies**:
    - **Rolling update** \(default\)
    - **Recreate**

## Limits and Requests
-  You can set limits and request for container 
- **Request determines how pods will be scheduled** 
- **Limits enusure that Pod is not consuming too much resources**
    -  **Exceeding CPU** limit will result in **throttling**
    -  **Exceeding memory** will result in **eviction**

## Taints & Tolerations
-  Taints are forbidding nodes to accept pods
    -  when **taint is applied** to the node, **only pods with tolerations** cab scheduled to pod 
    -  taint\-effects
        - **NoSchedule** \- new pods won't be scheduled
        - **PreferNoSchedule** \- new pods won't be scheduled if possible
        - **NoExecute** \- new pods won't be scheduled to the node, and **currently scheduled pods will be evicted**
- Note: it only limits which pod can be scheduled to the node, but **does not guarantee that pod with tolerations will be scheduled to node with taint**

## Node Selector & Node Affinity
-  Node selectors can be used to **select the specific node** where pod should be scheduled
-  Affinity \- more advanced mechanism for controlling how pods will be scheduled
    -  can be applied **on Node level \(Node Affinity\) or Pod level** \(Pod affinity \- pods will be scheduled on the same node\)
-  For pods it can be also pod anti\-affinity 
-  types of affinity:
    - **requiredDuringSchedulingIgnoredDuringExecution**
    - **preferredDuringSchedulingIgnoredDuringExecution** 
    - **requiredDuringSchedulingRequiredDuringExecution**

## Multi\-container Pods
- **Ambassador** \- eg. database proxy
- **Adapter** \- eg. log processing before sending it to log server
- **Side car** \- eg. logging server aside to microservice
- **Init container** \- containers being run on pod creation, to prepare pod environment eg. creating directories etc.

## Observability
-  Pod phases
    -  Pending
    -  Running
    -  Succeeded
    -  Unknown
    -  Failed
-  Pod condition \(true/false\)
    -  PodScheduled
    -  ContainersReady
    -  Initialized
    -  Ready
-  Container statuses
    -  Waiting
    -  Running
    -  Terminated
-  restartPolicy
    -  Always
    -  OnFailure
    -  Never
-  Probe types
    - **liveness probe** \- if failed **pod will be restarted**
    - **readiness probe** \- if succeed traffic **will be routed to pod**
    -  Actions
        - httpGet
        - tcpSocket
        - exec

##  Metric server

-  Basic monitoring tool in K8S
-  In\-memory storage
-  Kubelet \(or rather its cAdvisor component\) reports metrics to Metric server

## Authentication, Authorization & Admission Control

- **Authentication** \- Who can access the cluter?
    -  Files \- username and passwords \(not recommended\)
    -  Files \- username and tokens \(not recommended\)
    -  LDAP/Kerberos
    -  Certificates 
    -  Service accounts
-  Authorization \- What they can do?
    -  RBAC \- users are being assigned with roles and permissions
        - Roles that are grouping permissions
        - Roles can be assigned to users and groups
        -  You need to specify
            -  Role
                - **apiGroups** \- core, ...
                - **resources** \- pod, configmap
                - **verbs** \- eg. list, get...
                -  \(optionally\) **resource names**
        -  Role binding
            -  Binding role with user or user group
            -  Binding is associated with the namespace 
        -  Cluster roles \- used to authorize user to alter cluster scope resources 
            -  Cluster roles can be created also for namespaced resources \- then user will have such access across all namespaces
            -  Cluster Scoped resources
                -  nodes
                -  PV
                -  clusterroles
                -  clusterrolebindings
                -  namespace
                -  certificatesigningrequests
    -  ABAC\- Attribute based authorization
        -  Policy file \(json\) which is defining permissions for specific user or group
        -  Such policy file has to be passed as parameter to api\-server
    -  Node authorizers 
        -  Used to authorize nodes across the cluster based on certificates
    -  Webhooks
        -  Mechanism for delegating responsibility of authorising action for a user to external entity
    -  AlwaysAllow/AlwaysDeny
    -  If multiple Authorization does are configured they are used in the order specified
-  Communication between Api Server and all other components is secured with TLS

## Administration
- **kubeadm** \- command line tool for creating K8S clusters
- **kubeconfig**
    -  clusters \- various clusters definitions
        -  server url
        -  certificate\-authority
        - ...
-  users \- various users configuration
    -  client\-key
    -  client\-certificate
-  contexts \- binds clusters and users which can be used later on
    - optionally you can set up namespace for the context

## API Groups
-  Endpoints
    -  /metrics
    -  /healthz
    -  /version
-  /api \- core APIs
    -  /pod
    -  /namespace
    -  ...
-  /apis \- named
    -  /apps
    -  /networking.k8s.io
    -  /certificates.k8s.io
-  /logs
- kubectl proxy \- proxy to cluster API which will append kubectl credentials
    -  It's not the same as kube proxy \- proxy allowing connection between resources on different nodes of the cluster

## Admission controller
- Validation mechanism that provides possibility to enforce some properties of object being deployed on the cluster
-  Default admission controllers:
    -  AlwaysPullImages
    -  DefaultStorageClass
    -  EventRateLimit
    -  NamespaceExists
- To enable some Admission controllers you need to add it to /etc/kubernetes/manifests/kube\-apiserver.yaml config file by adding it to \-\-enable\-admission\-plugins switch
-  You can disable admission controllers by adding it to \-\-disable\-admission\-plugins switch
-  Admission controller types:
    - **Mutating admission controller** \- admission controller which is changing the object before its creation
    - **Validating admission controller** \- admission controller which are validating the object before its creation
- Mutating admission controllers should be run before validation admission controllers
-  There might be admission controllers which are doing both things
-  You can develop your own admission controllers based on webhooks

## API versions
-  Resources can be versioned on different APIs
    -  alpha
        -  first time when API is added to K8S
        -  not added by default
    -  beta 
        -  major bugs fixed
        -  e2e test finished
    -  GA/Stable \(eg. simply v1\)
-  Multiple versions can be used in the same time for the ApiGroup
- Only one version can be set to preffered \(used for querying object with kubectl\)
-  Only one version can be set to storage \(version in according to which objects are saved in etcd\)
-  To enable or disable api you need to alter \-\-runtime\-config switch for kube\-apiserver

## API deprecation
1. API elements may only be removed by implementing  another version of the API group
2. API objects must be able to round\-trip between API versions in a given release without information loss
3. API version in a given track cannot be deprecated until a new API version at least as stable is released
4. Rule 4
    1. Other than most recent API version in each track, older API versions must be supported after their announced deprecation for duration of no less than:
- GA: 12 months or 3 releases
- Beta: 9 months
- Alpha: 0 releases
    2.  The preferred and storage API version cannot be advanced before next release after the one which both versions new and deprecated one are being released

You can use \`kubectl convert\` to convert your yaml files definitions between different API version

## Custom Resources

- **Controller** \- \(just another\) **process or container** that is **observing specific objects in etcd** and **taking care of making a specific actions** if such object will be created
-  To be able to create some customer resources you need to create **CustomResourceDefinition** first
    -  schema needs to be defined according to openapiv3 spec
- **Operator \- encapsulates CRD and Custom Controller into one object**

## Deployment strategies
- **Canary** \- simply deploy new deployment with only one replica
- **Blue/Green** \- simply deploy another deployment and switch traffic by changing selector

## Capacity planning
_Based on openshift recommendations_
- Maxiumum recommended pod dencity is 10 pods / core 
- Number of Nodes =  Max pods per cluster / Expected pods per node
- Overcommiting 
    - allowing single node would handle number of pods with summary limits exceding pod capacity assuming that not every code will be using it's max CPU load
    - you can control overcommiting ratio \- meomoryRequestToLimitPercent, cpuRequestToLimitPercent
- QOS strategies
    - Guarnateed \- Request = Limit
    - Burstable \- Request \< Limit
    - Best effort \- request and limit are not defined
- Eviction
    - appears when RAM or Disk usage is overreached
    - Can be hard, or soft \(giving a Pod some time to gracefully shutdown\)
- Setups
    - All in one
    - Single master \+ Nodes
    - Multiple masters in HA mode \(\+ Load balancer on separate node if not in the cloud\) 
- Quotas
    - Can limit size of whole namespaces
    - You may create them from the t\-shirt sizes
- Recommendations for reserving kube and system resources:
    - Kube\-reserved cpu: 5m x max number of pods per node
    - Kube\-reserved memory: 5M x max number of pods per node
    - System\-reserved cpu: 5M x max number of pods per node
    - System\-reserved memory: 10M x max number of pods per node
- ![[Pasted image 20230217123051.png]]

## High Availability
- ...

## Securing  K8S cluster
1. Keep Kubernetes up to date
2. Secure API Server with OpenID Connect:
    - it's much easier to manage users
    - no credentials are stored on users computer
    - it's easy to revoke access
![[image.png]]
3. Secure Kubernetes API Server and etcd with TLS \(might be mutual TLS\)
4. Enable encryption at rest for etcd
5. Use RBAC
    1. Enable RBAC
    2. Use one service account per application
    3. Configure permissions with least privilege priciple in mind
    4. Reduce API server flags like  \(?\)
        1. \-anonymous\-auth
        2. \-insecure\-bind\-address
        3. \-insecure\-port
6. Control access to the kubelet
    1. Disable anonymous access
    2. Enforce api\-server auth
        1. \-\-kubelet\-client\-certificate
        2. \-\-kubelet\-client\-key
    3. Close the read\-only port
    4. Add `NodeRestrictions` in API server \-admission\-control setting, so kubelet can change only the pods on his node
7. Use admission contollers to enforce security good practices on pods
    - eg. ImagePolicyWebhook to allow only images which hasn't been scanned etc.
8. Facilitate greater control over networking with service mesh
    - Applying mTLS
    - Ingress control
9. Consider to run security sensitive applications on separated nodes
10. Set namespaces and networking policies
11. Enable audit logging
12. Secure nodes according to general security best practices
13. You can observe network traffic and processes to look for anomalies
14. Use Security contexts to manage linux capabilities etc.
15. You can apply advanced network security controls with tools like Calico or Wavenet \(L3\-4\) or Kubernetes proxy like Envoy \(L7\)
16. Enable mutual TLS  with service mesh
17. Apply IDS and IPS solutions
    - Analyze network traffic
    - Leverage machine learning to detect anomalies
    - Use threat intelligent \- leverage databases of known malicious IPs
18. Apply CIS kubernetes benchmark
19. Secure K8S dashboard
    - Limit capabilities of dashboards service account
    - Introduce reverse proxy with OIDC
    - Use RBAC
20. Allow only Trusted Images and scan them
21. Consider container sandboxing for creating a lightweight VM for isolating containers from host kernel
    - eg. Kata Containers, gVisor, Firecracker


---

# Helm
-  Vars \+ Template = Chart

---

# Other
### [Brigade](https://brigade.sh/)
Scripting.. tool.. framework.. simplifing many DevOps / Admin tasks on Kubernetes.. good for:
- Backups
- ETL
- Cron tasks
- Continuous ingegration and testing
- Chat ops

## Kustomize

## Skaffold

## ArgoCD