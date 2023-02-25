---
title: Docker
creation_date: 2023-02-17
tags:
	- IT
	- SysOps
	- Docker
---

# Docker
## Architecture
- Docker CLI
- Docker Daemon
	- containerd - deamon responsible for lifecycle of the containers
		- runc - abstration layer for single container, responsible syscall to the kernel and managing cgroups, namespaces, sec capabilities etc.

OCI - Open Container Initiative - organization responsible for creating standard for Containers and Images specification

## Creating Dockerfiles
-  Every command in Dockerfile is another layer
-  If you are changing the Dockerfile and rebuilding the image, untouched layers will be taken from cache
- layers which are chaning most often should be put on top
- user shouldn't be root

### Specifc commands
- **ENTRYPOINT**\- **can be appended** by parameters from command line
- CMD \- simple command, **can be overwritten** form the command line
- ENTRYPOINT \+ CMD \- you can **specify entrypoint command as** **ENTRYPOINT** and **default paramaters as** **CMD** which **can be overwritten** by parameters from command line

## Multistage builds
- You can bulild app in temporary image and then extract artifact into another thin image

## Building secure docker images
1. Lint Dockerfiles
    - USER directive is specified
    - Image version is pinned
    - OS packages versions are pinned
    - Use COPY instead of ADD
        - ADD allows to add resource also directly form the url
    - Avoid curl in RUN directives
    - Detect secrets in images
        - ggshield
        - SecretScanner
2. Do not use root user
    1. Do not bind specific uid either
3. Use minimal \(distroless\) os Images
    1. Use multistage build in order to not include any build tools etc
4. Use DCT \(Docker content Trust\) to verify base images
5. Make executables owned by root user
    - All executables should be owned by root user and should be not world\-writable
6. Do not expose ports which are not needed 
7. Scan for container vulnerabilities
    1. [[Clair]]
    2. [[Trivy]]
    3. [[Threat Mapper]]
    4. [[JFrogXray]]
    5. [[anchore]]
    6. [[Snyk]]
8. Install packages only from trusted repositories
9. Remove setuid and setgid permissions from the image
10. Add HEALTCHECK command to the container
11. Apply CIS Docker benchmark

## Docker security
There are 4 aspects of docker security:
1. Security of the kernel and its support for namespaces and cgroups
2. Attack surface of the Docker daemon itself
3. Loopholes in the container configuration profile
4. Hardening security features of the kernel and how they interact with containers

### Considerations
- Each container gets its own namespaces \(it cannot see and alter any processes in other namespaces\) and its own network stack \(it can't get privileged access to the sockets or interfaces of another containers\).
- All containers on Docker host are sitting on bridge interfaces \(they are like physical machines connected via one Ethernet switch\).
- If you would use Docker links or publish container port then container can talk to each other. 
- Every user of Docker daemon can share host `/`  directory with the container so the container can alter host filesystem without any restrictions
- Linux capabilities allow to limit what can be done after escalating to root for example:
    - deny all 'mount' operations
    - deny access to raw sockets \(to prevent packet spoofing\)
    - deny access to some filesystem operation, like creating new device nodes changing file ownership etc
    - deny module loading
- Docker scan runs on [[Snyk]] engine 
- Docker denies all capabilities by default but they can be enabled for the specific containers 

## Securing docker daemon

1. Keep host and docker engine up to date 
2. Do not expose Docker daemon socket \(even to the containers\)
    - It shouldn't be exposed via [[TCP]]
        - In general it's possible to expose it with [[SSH]] or [[TLS]]
    - /var/run/dock.sock shouldn't be mounted into containers
3. Allow only trusted users to use docker daemon
4. Remap user namespace for containers so even if privileges would be escalated to root still on host it would be a regular user
    - Ranges shouldn't overlap so container wouldn't be able to hijack other container's user
5. Run docker in rootless mode
    - Both daemon and containers will be run in the separate namespace
6. Run processes in the containers by specific user \(not root\) 
    - Can be done while running container \(docker run \-u 4000\)
    - Or during build time
    - Enable user namespace support
    - Do not bind the specific UID
        - Container orchestrators like Openshift are running containers with random uid\) 
    - You need to ensure that the process will have access to certain directories where it operates
7. Limit [[SELINUX]] capabilities
    - Do not run containerrs with \-\-privilaged flag \- it would add all capabilities
    - You can use \-\-cap\-drop to drop specific  capabilities or drop them all \-\-cap\-drop all
8. Run containers with \-\-no\-new\-privilages flag
    - This prevents to escalate privileges with setuid and setgid binaries
9. Disable inter\-container communication
    - run docker daemon with \-\-icc=false flag
    - some specific containers can still be allowed to communicate if configured explicitly
10. Segregate Docker networking
    - Containers should be able to talk to each other only if necessary
    - Do not use default bridge network
    - Create multiple networks for different purposes
11. Use Linux Security Module \- you can configure what application can do \(syscalls, access to systems resoruces like files or network\) with great granularity and differentiate it between many containers
    - [[seccomp]] \- kernel feature which allows to filter calls to the kernel form a containers \(more fine grained control than capabilities\)
    - [[AppArmor]] \- Mandatory Access Control tool like [[SELinux]]
    - [[SELinux]]
12. Limit Resources for the containers \- it will protect you from DoS attack
13. Set filesystem to red only mode
    - If container needs something temporarily then mount a tmpfs 
14. Use static analysis tools
    - Detecting container vulnerabilities
        1. [[Clair]]
        2. [[Trivy]]
        3. [[Threat Mapper]]
        4. [[JFrogXray]]
        5. [[anchore]]
        6. [[Snyk]]
    - Detect secrets in images
        1. [[ggshield]]
        2. [[SecretScanner]]
    - Detect misconfiguration in Kubernetes
        1. kubedit
        2. [kubesec.io](http://kubesec.io)
        3. kube\-bench
    - Detect misconfiguration in docker
        1. [inspec.io](http://inspec.io)
        2. [dev\-sec.io](http://dev-sec.io)
        3. Docker Bench for Security
15. Ensure proper docker daemon logging
    1. Ensure INFO level is enabled
    2. Do not enable DEBUG unless necessary

