
### Demo - Deployment with Kubeadm

#### Kubernetes documentation links to follow
- https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/
- https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/
- https://kubernetes.io/docs/setup/production-environment/container-runtimes/
- https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/high-availability/

#### Install kubeadm, kubelet and kubectl
- https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/#installing-kubeadm-kubelet-and-kubectl
- `kubeadm version` -> Check Kubeadm installation

#### Creating cluster with Kubeadm
- https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/
- Set forwarding rules (sysctl) - https://kubernetes.io/docs/setup/production-environment/container-runtimes/#install-and-configure-prerequisites
- Install CRI on all nodes - https://kubernetes.io/docs/setup/production-environment/container-runtimes/#containerd
	- `sudo apt update`
	- `sudo apt install -y containerd`
- Configure `systemd cgroup` driver - https://kubernetes.io/docs/setup/production-environment/container-runtimes/#containerd-systemd
	- Check `init` system on the VM - `ps -p 1` -> If it is `systemd` or `cgroupfs`
	- Once confirmed that `systemd` is used, configure the `systemd cgroup` driver - https://kubernetes.io/docs/setup/production-environment/container-runtimes/#containerd-systemd
	- On all VMs
		- `sudo mkdir -p /etc/containerd`
		- `containerd config default | sed 's/SystemdCgroup = false/SystemdCgroup = true/' | sudo tee /etc/containerd/config.toml`
		- `sudo systemctl restart containerd`
- Initialize control plane node - https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/#initializing-your-control-plane-node
	- Run on master node - `sudo kubeadm init --apiserver-advertise-address <ip_of_master_node> --pod-network-cidr "<any_cidr/16>" --upload-certs`
		- Default pod network cidr is `10.244.0.0/16`
		- If you change this, make sure to change when creating a CNI (later).
	- Once the command finishes successfully, run the command specified in the console to configure permissions for user to talk to the kube-apiserver.
	- Note down the command on the console to join worker nodes to the cluster
- Install CNI plugin / pod network addon in master node
	- https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/#pod-network
	- https://kubernetes.io/docs/concepts/cluster-administration/addons/#networking-and-network-policy
	- https://github.com/flannel-io/flannel#deploying-flannel-manually
	- `kubectl apply -f https://github.com/flannel-io/flannel/releases/latest/download/kube-flannel.yml`
	- If pod cidr was changed to something other than default (10.244.0.0/16), then download the yaml file, change the cidr and run the `kubectl apply ...` command.
	- `kubectl get nodes` should show that the master node to be in ready state
- Join the worker nodes to cluster
	- Use the command from the output of initializing the master node on all worker nodes.
	- https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/#join-nodes
	- https://kubernetes.io/docs/tasks/administer-cluster/kubeadm/adding-linux-nodes/
	- `kubectl get nodes` should show that all nodes to be in ready state
- Test
	- `kubectl run web --image=nginx`
		- All should be running.


---



