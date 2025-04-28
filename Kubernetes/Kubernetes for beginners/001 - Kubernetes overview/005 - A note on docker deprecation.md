
### Docker deprecation

- Docker consists of multiple tools / components put together like,
	- CLI
	- API
	- Build
	- Volumes
	- Auth
	- Security
	- ContainerD
		- Container runtime called "runc" and the daemon that managed the container called ContainerD
- ContainerD is CRL compatible and can work with kubernetes
- ContainerD can operate on its own, separate from docker.
- Kubernetes did not need other tools which docker supported like, CLI, API, Build etc, as it was taken care by kubernetes itself.
	- `docker` command equivalent is `nerdctl` for ContainerD.


---
