
### Container Network Interface

As per CNI
- Container runtime must create network namespace
- Identify and attaching the network which the container must attach to
- Container runtime to invoke network plugin (Bridge), when the container is added
- Container runtime to invoke network plugin (Bridge), when the container is deleted
- JSON format for network configuration

Configuring CNI
- CNI plugin must be invoked by the component responsible for creating containers, which is the container runtime, like Kubernetes, CRIO etc
- CNI plugins are installed in `/opt/cni/bin/` - CNI supported plugin binaries
- Different CNI plugins
	- Weave works
	- VMware NSX
	- Flannel
	- Cilium
- Which CNI plugin to use is mentioned under `/etc/cni/net.d`
- ![cnisamplefile.png](Attachments/cnisamplefile.png)
- To delete a CNI
	- `kubectl delete daemonset -n kube-flannel kube-flannel-ds` - Deletes the daemon set
	- `kubectl delete cm kube-flannel-cfg -n kube-flannel` - Deletes config maps
	- `rm /etc/cni/net.d/10-flannel.conflist` - Config file delete

---
