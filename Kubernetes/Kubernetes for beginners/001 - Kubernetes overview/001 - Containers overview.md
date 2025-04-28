
### Containers overview

Problems with traditional environment
- Multiple softwares like NodeJS for frontend, MongoDB for database, Redis for messaging and caching,  Ansible for orchestration make up an application.
- To run these softwares on a OS gives raise to dependency with underlying OS.
- Dependency on other libraries and 3rd party softwares leading to potential incompatibility
- Matrix from hell
	- Upgrade of OS might break compatibility
	- Upgrade of 3rd party OSS and libraries might break compatibility
	- Upgrade of software versions might break compatibility
- On-boarding a new developer to setup environment is cumbersome and prone to errors.
- Setting up and maintaining different environments, like test, dev and prod, is cumbersome and prone to errors.

Docker solves the above problems
- In Docker, Each software is run in its own environment, which is commonly known as Container.
- Each container has its own libraries and dependencies, without depending on or affecting other softwares or containers.
- Underlying OS is also abstracted from the software running inside the container.
- Build the docker configuration once, share it with others to deploy using `docker run` command.
	- This will work irrespective of their work environment or knowledge of application.

**Containers**
- Completely isolated environments having their own processes, networks and mounts.
- ![containers.png](Attachments/containers.png)
- Main purpose of Docker is to containerize applications, and to ship them and run them.
	- P.S: Cannot run Windows based containers on a Linux based Docker host.
- Containerized applications are available in public docker registry called Docker Hub.
	- $`docker run nodejs` downloads the `nginx` image from Docker Hub and runs the container.
- **Docker Image**
	- Is a package or template to create one or more containers
- **Docker Containers**
	- Running instances of images that are isolated, which have their own environment and set of processes.

**Virtual Machines**
- ![vms.png](Attachments/vms.png)
- Higher consumption of CPU, memory and disk space compared to Containers.
- VMs have higher boot times compared to Containers
- Docker has less isolation compared to VMs.


---

