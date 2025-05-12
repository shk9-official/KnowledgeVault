

### Kubelet

- Kubelet on the worker node registers the node on the Kubernetes cluster
- When a kube-scheduler identifies a pod to be deployed on a node, it instructs kube-apiserver
- kube-apiserver instructs kubelet to deploy the pod in its node
- kubelet instructs its container runtime engine, like containerD, to pull the image and run an instance
- kubelet continues to monitor the pods and nodes, and reports the status back to kube-apiserver
- You can manually install and configure kubelet on the master node, by downloading the corresponding binary 
- Service - kubelet.service
- Kubeadm does not automatically deploy kubelets.
- kubelets need to be installed manually on worker nodes
- $`ps -aux | grep kubelet`


---
