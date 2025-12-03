
### Worker node failure

- Check status of nodes in cluster, ready/not ready
	- `kubectl get nodes`
	- In the output of `kubectl describe node <name_of_node>` for a healthy node
		- Ready should be set to true
		- Everything else should be set to false
- Check logs of node
	- `top`
- Check disk
	- `df -h`
- Check kubelet status
	- `service kubelet status/start/stop/restart`
- Check kubelet logs
	- `journalctl -u kubelet`
- Check kubelet certificates
	- `openssl x509 -in /var/lib/kubelet/worker-1.crt -text`
- Check status of kubelet service. If down, bring it up
- View/edit configuration of kubelet service
	- `/etc/kubernetes/kubelet.conf`
	- Control plane ip and port (6443) should be correct
- View/edit kubelet configuration for worker nodes
	- `/var/lib/kubelet/config.yaml`
	- Check for certificate file path and misconfigurations here
- Restart kubelet service is these 2 files are changed

![Kubernetes-CKA-1000-Troubleshooting-1.pdf](Attachments/Kubernetes-CKA-1000-Troubleshooting-1.png)