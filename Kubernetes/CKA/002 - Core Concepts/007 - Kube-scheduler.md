
### Kube-scheduler

- kube-scheduler is responsible for deciding which pod goes to which node
	- kubelet then deploys the pod on that node
- Scheduler uses the following criteria to place pods on nodes
	- Filter nodes -
		- Based on resource requirement of a pod and the resource availability of a node
	- Rank nodes -
		- Ranks the nodes based on best fit, based on priority function and assigns a score from 0-10
- You can manually install and configure kube-scheduler on the master node, by downloading the corresponding binary 
-  $`kubectl get pods -n kube-system` - Lists the pod in which kube-scheduler is running
- Service - kube-scheduler.service
	- All configurations are here
- Pod definition file - /etc/kubernetes/manifests/kube-scheduler.yaml



---
