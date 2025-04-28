

### Introduction and installation

- Minikube and Microk8s can be used to setup kubernetes in laptop
	- To play around and learn kubernetes
- `kubeadm` tool is to bootstrap and manage production grade kubernetes cluster.
- Hosted solutions are available on GCP, AWS and Azure.

### Minikube and Kubectl

- Typically when installing kubernetes, Master and Worker nodes are created sperately as below
- ![master-workernodes.png](Attachments/master-workernodes.png)
- Minikube bundles all the above components into a single node kubernetes cluster
- Insert image here
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


### Pods

- Kubernetes encapsulates containers into an object called pod.
- Pod is an single instance of an application.
- Pod is the smallest object that can be created in kubernetes.
- A pod contains only one instance of an application
- To scale, we create new pods with instances of the application.
- If the node runs out of space, we bring in new node and create new pods with instances of the application
- Pod has 1-1 relationship with container it is running.
- ![pod-1.png](Attachments/pod-1.png)
- ![pod-2.png](Attachments/pod-2.png)
- A node will have many pods running.
- A cluster will have 1 or more nodes running.

**Multi-container Pod**
- If we need a helper container (like istio sidecar) to assist our application container, both of these can reside in the same pod.
- They share the same network and storage space.
- ![multicontainerpod.png](Attachments/multicontainerpod.png)

**kubectl**
- $`kubectl run nginx --image nginx`
	- The first mention of `nginx` can be replaces with any name
	- The second mention of `nginx` must match the image name in docker hub.
	- The above command
		- deploys a pod
		- deploys an instance of nginx application by downloading the image from docker hub
- $`kubectl get pods`
	- Lists pods in the cluster
- $`kubectl describe pod nginx`
	- Provides detailed information about pod, like name of the container, node name and IP of node, IP of pod, container id, events in pod etc.
- ![kubectlrungetpodsdescribepod.png](Attachments/kubectlrungetpodsdescribepod.png)
- $`kubectl get pods -o wide`
	- Provides a bit of additional information like node where the pod is running and IP of pod.
- ![kubectlgetpodswide.png](Attachments/kubectlgetpodswide.png)



---
