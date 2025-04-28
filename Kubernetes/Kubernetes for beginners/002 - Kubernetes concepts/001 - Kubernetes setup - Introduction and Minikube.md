

### Introduction and installation

- Minikube and Microk8s can be used to setup kubernetes in laptop
	- To play around and learn kubernetes
- `kubeadm` tool is to bootstrap and manage production grade kubernetes cluster.
- Hosted solutions are available on GCP, AWS and Azure.

### Minikube and Kubectl

- Typically when installing kubernetes, Master and Worker nodes are created sperately as below
- ![master-workernodes.png](Attachments/master-workernodes.png)
- Minikube bundles all the above components into a single node kubernetes cluster
- ![minikube.png](Attachments/minikube.png)
- Minikube install
	- ![minikubeinstall.png](Attachments/minikubeinstall.png)
	- Minikube start without docker
	- ![minikubestartwithoutdocker.png](Attachments/minikubestartwithoutdocker.png)
	- Minikube start and status $`minikube start` and $`minikube status`
	- ![minikubestartstatuskubectlgetnodes.png](Attachments/minikubestartstatuskubectlgetnodes.png)
- Kubectl install
	- ![kubectlinstall.png](Attachments/kubectlinstall.png)

### Kubectl

- $`kubectl get nodes`
- $`kubectl create deployment hello-minikube --image=kicbase/echo-server:1.0`
- $`kubectl get deployments`
- ![kubectlcreatedeployment.png](Attachments/kubectlcreatedeployment.png)
- $`kubectl expose deployment hello-minikube --type=NodePort --port=8080`
- $`kubectl get services hello-minikube`
- ![kubectlexposedeploymentgetservices.png](Attachments/kubectlexposedeploymentgetservices.png)
- $`kubectl delete deployment hello-minikube`
- $`kubectl delete services hello-minikube`
- ![kubectldeletedeploymentservice.png](Attachments/kubectldeletedeploymentservice.png)


---
