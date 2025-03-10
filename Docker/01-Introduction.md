### Docker overview

Assume a stack with the following
- Webserver - NodeJS
- DB - MongoDB
- Messaging - Redis
- Orchestration - Ansible

**Problems running the above stack on traditional VMs**
- Compatibility matrix problem
	- Compatibility with underlying OS, like application versions.
	- One service might require a.bc version of the library and the other service might require x.yz version of the same library.
- Onboarding a new developer is a big problem.
- Difficulty in porting application to different environments like development, testing etc.

**Solution**
 - Containerise each application to run in sylos, like in a sandboxed environment.
 - Containerising it makes applications independent of underlying OS and other applications.
	 - Ex: NodeJS will run as separate container, MongoDB as a separate container etc.
 - Each container will satisfy its own dependencies.

**Containers**
- Separate process, networking, mounts etc.
- Hardware -> OS -> Docker -> Container 1, Container 2, Container 3 etc.
- Size of containers are in MBs, whereas size of VMs are in GBs.
- Containers CPU utilisation is comparatively low to that of VMs.
- Containers boot fast compared to VMs.

Public docker registry -> docker hub (hub.docker.com)

Docker images -> Package or template used to launch docker containers.

Docker containers -> Running Docker images.

---

