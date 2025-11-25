
### Setup VMs on Virtual box via Vagrant

- Install virtual box
- Install Vagrant
- Use vagrant file from https://github.com/kodekloudhub/certified-kubernetes-administrator-course/blob/master/kubeadm-clusters/virtualbox/Vagrantfile
	- 1 master node and 2 worker nodes
	- `vagrant status` gives status of all VMs specified in the `Vagrantfile`
	- `vagrant up` will setup all the VMs specified in the `Vagrantfile`
- Connect to the VMs
	- `vagrant ssh <VM_Name>`
	- `vagrant ssh kubemaster`
	- `vagrant ssh kubenode01`
	- `vagrant ssh kubenode02`


---
