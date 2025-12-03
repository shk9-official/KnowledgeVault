
### Control plane failure

- If control plane components are deployed as pods
	- `kubectl get pods -n kube-system`
- If control plane components are deployed as services
	- `service kube-apiserver status/stop/start/restart`
	- `service kube-controller-manager status/stop/start/restart`
	- `service kube-scheduler status/stop/start/restart`
	- `service kubelet status/stop/start/restart`
	- `service kube-proxy status/stop/start/restart`
- Logs of control plane components
	- `kubectl logs kube-apiserver -n kube-system`
	- `sudo journalctl -u kube-apiserver`
- https://kubernetes.io/docs/tasks/debug/debug-cluster/
- If a pod is in pending state, it means it is not assigned to a node
	- It might be because the kube-scheduler is down
	- Check the kube-scheduler pod's event, command, arguments and environment variables using the describe command
	- Edit changes to kube-scheduler in /etc/kubernetes/manifests/kube-scheduler.yaml
- Job of scaling up/down a deployment/replica set is the kube-controller-manager's responsibility
	- If you scale up a deployment, but don't see the increase in the number of pods, then the kube-controller-manager might be down
	- Check the kube-controller-manager pod's event, command, arguments and environment variables using the describe command
	- Check if volume and volume mounts are correct
		- Check if file paths exist and are correct
	- Check the logs of kube-controller-manager pod
	- Edit changes to kube-controller-manager in /etc/kubernetes/manifests/kube-controller-manager.yaml
- If kube-proxy service is down, edit kube-proxy, via the daemon set
	- `kubectl edit ds -n kube-system kube-proxy`


---
