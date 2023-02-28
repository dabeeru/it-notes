- Build for [[Kubernetes]]

## Features
- Can spport mutiple clusters
- Drift detection and visualization
- Continouously monitors the state of infrastructure and allows to manually or automatically remediate it's state
- PreSync, Sync, PostSync hooks

### Other
- SSO integration
- Multi-tenancy and RBAC

## Configuration
- [[Kubernetes]] manifests can be stored as
	- [[kustomize]]
	- [[helm]] charts
	- [[jsonnet]]
	- plain [[yaml]]
	- custom config managment tool configured as config management plugin

## Tracing stragies
### Auto-sync
- Change is deployed as soon as detected

### Helm
Based on [[SemVer]]:
- Pin to version
- Track patches 
- Track minor releases
- Use the latest

### Git
Based on tags or sha:
- Pin to a version 
- Track patches
- Track minor release
- Use latest