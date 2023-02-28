

- ECS \- managed orchestration service
- You can create clusters to manage fleets of container deployments
- ECS manages **EC2** or **Fargate** instances
- You can **define CPU** and **memory requirements**
- **Basic monitoring** available
- You are paying only for **compute resources**

### Components

- **Cluster**  logical collection of ECS resources ECS **EC2** instances or **Fargate**
- **Task**  **Single running copy of any container** defined by task definition
- **Task Definition**  **Definition of container** \(or **multiple containers**\)
- **Service**  Allows **task definitions** to be **scaled by adding tasks**, **defining minimum** and **maximum values**
- **Container Definition**  i**nside task definition**, it defines the individual containers a task uses, controls **CPU, memory and port mappings**
- **Registry**  Storage for container images

### Integration with ELB

- Works with ALB NLB and CLB

### Security

- If role is assigned to EC2 then all containers inherit it
- Role can be assigned to the task \- preffered
