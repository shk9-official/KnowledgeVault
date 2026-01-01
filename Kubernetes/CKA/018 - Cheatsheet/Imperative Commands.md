
Get pods
- `kubectl get pods -o wide -A`
- `kubectl get pods -o wide -n default`
- Selectors in get pods command
	- `kubectl get pods --selector app=App1`

Create pod
- `kubectl run nginx --image=nginx -n=<name_of_namespace> --labels="tier=db" --port=8080 --dry-run=client -o yaml > nginx-pod.yaml`
- `kubectl apply -f nginx-pod.yaml`

Describe pod
- `kubectl describe pod <pod_name>`

Delete pod
- `kubectl delete pod <pod_name>`

Edit a pod
- `kubectl edit pod <pod_name>`

Expose pod on a port
- `kubectl expose pod redis --port=6379 --name=redis-service`
	- `kubectl expose pod redis --port=6379 --name redis-service --dry-run=client -o yaml` -> Automatically use pod's labels as selectors, but cannot specify NodePort
- `kubectl expose pod httpd --port=80 --name=httpd --type=clusterip`
- `kubectl run httpd --image=httpd:alpine --port 80 --expose` - > Create a pod and expose in same command
- `kubectl expose pod nginx --type=NodePort --port=80 --name=nginx-service --dry-run=client -o yaml` -> Automatically use pod's labels as selectors, but cannot specify NodePort

Get replication controllers
- `kubectl get replicationcontroller`

Get replica sets
- `kubectl get replicaset`

Describe replica set
- `kubectl describe replicaset <replicaset_name>`

Delete replica set
- `kubectl delete replicaset <replicaset_name>`

Explain replica set
- `kubectl explain replicaset`

Increase number of replicas in replica set
- `kubectl scale --replicas=6 replicaset <name_of_replicaset>`
- `kubectl edit replicaset <name_of_replicaset>` --> edit the number of replicas

Get deployments
- `kubectl get deployment`

Get all Kubernetes objects
- `kubectl get all`

Describe deployment
- `kubectl describe deployment <deployment_name>`

Create deployment
- `kubectl create deployment <deployment_name> --image=<container_image_from_dockerhub> --replicas=<number_of_replicas> -n=<name_of_namespace>`
- `kubectl create deployment <deployment_name> --image=<container_image_from_dockerhub> --dry-run=client -o yaml > deployment-definition.yaml`
	- `kubectl create -f deployment-definition.yaml`

Status of rollout
- `kubectl rollout status <deployment_name>`

Revisions and history of rollouts
- `kubectl rollout history <deployment_name>`

Change container image in a deployment
- `kubectl set image <deployment_name> <container_name>=<image_name>`

Rollback deployment revision
- `kubectl rollout undo <deployment_name>`

Edit deployment
- `kubectl edit deployment <deployment_name> --record`

Scale deployment
- `kubectl scale deployment nginx --replicas=5`

Set deployment image
- `kubectl set image deployment nginx nginx=nginx:1.18`

Expose deployment on a port
- `kubectl expose deployment nginx --port 80`

Create namespace
- `kubectl create namespace <name_of_namespace>`

Switch to another namespace
- `kubectl config set-context $(kubectl config current-context) --namespace=<name_of_namespace>`

Get namespaces
- `kubectl get namespaces`

List all api resources
- `kubectl api-resources`
- `kubectl api-resources --namespaced=true` -> Namespaced resources
- `kubectl api-resources --namespaced=false` -> Cluster scoped resources

Explain a resource
- `kubectl explain <resource_name_like_pod/deploy/etc>`
- `kubectl explain deploy.spec.replicas` -> Provides specific details about a parameter in the yaml field
- `kubectl explain <resource_name_like_pod/deploy/etc> --recursive` -> Explains all fields present in detail

Get services
- `kubectl get services`

Create NodePort service
- `kubectl create service nodeport webapp-service --tcp=8080:8080 --node-port=30080 --dry-run=client -o yaml > service-definition.yaml` -> Will not have pod's labels as selectors, but can specify nodeport.
- `kubectl create -f service-definition.yaml`
- `kubectl create service --help`
- `kubectl create service nodeport --help`

Describe service
- `kubectl describe service <service_name>`

Create ClusterIP service
- `kubectl create service clusterip redis --tcp=6379:6379 --dry-run=client -o yaml > service-definition.yaml`

Get all objects
- `kubectl get all --selector env=prod`
- Get the number of pods having label `env=dev`
	- `kubectl get pods --selector env=dev --no-headers | wc -l`

Taint nodes
- `kubectl taint nodes <node_name> key=value:taint-effect`
- `kubectl taint nodes node01 app=blue:NoSchedule`
- `kubectl taint nodes node01 app=blue:PreferNoSchedule`
- `kubectl taint nodes node01 app=blue:NoExecute`

Get nodes
- `kubectl get nodes`

Describe node
- `kubectl describe node kubemaster`
- `Labels:` section will have the labels assigned to the node

Un-taint nodes
- `kubectl taint nodes node01 app=blue:NoSchedule-`
- `kubectl taint nodes controlplane node-role.kubernetes.io/control-plane:NoSchedule-`

Label nodes
- `kubectl label nodes <name_of_node> <label_key>=<label_value>`

Get daemon sets
- `kubectl get daemonsets -A`

Describe daemon sets
- `kubectl describe daemonset <name_of_daemonset> -n <name_of_namespace>`

Create static pod
- `kubectl run static-busybox --image=busybox --dry-run=client -o yaml --command -- sleep 1000 > static-busybox.yaml`
- Copy yaml file to `/etc/kubernetes/manifests` 
	- This path can be specified in `kubelet.service` under `--pod-manifest-path` parameter or in a file mentioned under `--config` parameter.
		- In the file mentioned under `--config` parameter, the path is specified as `staticPodPath:<path>`

Identify static pods
- `kubectl get pod <name_of_pod> -n <name_of_namespace> -o yaml`
    - See the `OwnersReferences:` section - For static pods, it will be `kind:Node`

To identify directory path where the definition files are stored
- Run `ps aux | grep kubelet`
- Get the `--config` value, which will point to a yaml file
- In the yaml file, see the path set for `staticPodPath:`. This will give the directory location where manifest/definition files are stored

List static pods
- `kubectl get pods -A` and `kubectl get nodes`
    - Look for pods with `-<name_of_node>` appended to its name

Delete static pod
- SSH into the node
- Get the config file location by running `ps aux | grep kubelet` at `--config`
- Cat the config file and find the `staticPodPath:` location and remove the pod definition file which needs to be deleted (wait for 30 secs)
	- Or `/var/lib/kubelet` will also give `staticPodPath:` location

List priority class
- `kubectl get priorityclass`

Describe priority class
- `kubectl describe priorityclass <name_of_priorityclass>`

Create priority class
- `kubectl create priorityclass high-priority --value=100000 --description="high priority" --preemption-policy="PreemptLowerPriority"`
- `kubectl create priorityclass low-priority --value=1000 --description="low priority"`

Get priority class set on pods
- `kubectl get pods -o custom-columns="NAME:metadata.name,PRIORITY:spec.PriorityClassName"`

View scheduler scheduled runs or events
- `kubectl get events -o wide`

Get logs of a specific scheduler
- `kubectl logs <name_of_scheduler> --name-space=kube-system`

Get admission controller plugins enabled by default
- `kube-apiserver -h | grep enable-admission-plugins` or
- `kubectl exec -it kube-apiserver-controlplane -n kube-system -- kube-apiserver -h | grep enable-admission-plugins`

To enable admission controller plugins
- Modify `kube-apiserver.service`, which is present at `/etc/kubernetes/manifests/kube-apiserver.yaml`, at `--enable-admission-plugins` field.

To disable admission controller plugins
- Modify `kube-apiserver.service`, which is present at `/etc/kubernetes/manifests/kube-apiserver.yaml`, at `--disable-admission-plugins` field.

Get list of admission controllers enabled
- `cat /etc/kubernetes/manifests/kube-apiserver.yaml | grep enable-admission-plugins` or
- `ps -ef | grep kube-apiserver | grep admission-plugins`

Get list of additional admission controllers that are disabled
- `cat /etc/kubernetes/manifests/kube-apiserver.yaml | grep disable-admission-plugins` or
- `ps -ef | grep kube-apiserver | grep admission-plugins`

Install metrics server
- `kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml`

Get cluster metrics
- `kubectl top node`
- `kubectl top pod`

To view the logs/events which the pod has generated
- `kubectl logs -f <name_of_pod>`
- `kubectl logs -f <name_of_pod> <name_of_container>`

Specify arguments in kubectl command
- `kubectl run webapp-green --image=kodekloud/webapp-color -- --color green`
    - `--` separates options for `kubectl` utility and the options as arguments

Create ConfigMap
- `kubectl create configmap <config_name> --from-literal=APP_COLOR=blue --from-literal=APP_MODE=prod`
- `kubectl create configmap <config_name> --from-file=<path_to_file>`
	- `--from-file` allows you to specify the environment variables from a file like app_config.properties
	- ```app_config.properties
	  APP_COLOR: blue
	  APP_MODE: prod
	  ```

Get ConfigMaps
- `kubectl get configmaps` - Lists all ConfigMaps

Describe ConfigMaps
- `kubectl describe configmap <config_map>` - Describes a ConfigMap

Create Secret
- `kubectl create secret generic <secret_name> --from-literal=DB_Host=mysql --from-literal=DB_User=root --from-literal=DB_Password=paswrd`
-  `kubectl create secret generic <secret_name> --from-file=app_secret.properties`
	- `--from-file` allows you to specify the environment variables from a file like app_secret.properties
	- ```app_secret.properties
	  DB_Host: mysql
	  DB_User: root
	  DB_Password: paswrd
	  ```

Get secret
- `kubectl get secrets`

Describe secret
- `kubectl describe secret <secret_name>`

View secret value
- `kubect get secret <secret_name> -o yaml`
	- `echo -n <encoded_text> | base64-decode`

To view logs
- `kubectl -n <namespace_name> exec -it <pod_name> -- cat <log_path>`
- `kubectl logs <name_of_pod> -n <name_of_namespace>`
- `kubectl logs <name_of_pod> -n <name_of_namespace> -c <name_of_container>`

Manual way to horizontally scale a workload
- `kubectl scale deployment <name_of_deployment> --replicas=<increased_#_of_replicas>`

Configure HPA
- `kubectl autoscale deployment <name_of_deployment> --cpu-percent=50 --min=1 --max=10` -> Create the deployment first

List HPA
- `kubectl get hpa`

Delete HPA
- `kubectl delete hpa <name_of_deployment>`

Manual way to vertically scale pod
- `kubectl edit deployment <name_of_deployment>`

List VPA
- `kubectl get vpa`

Describe VPA
- `kubectl describe vpa <name_of_vpa>`
- `kubectl get vpa <name_of_vpa> -o yaml`

Get resource usages in pod
- `kubectl top pod`

Inspect VPA logs
- `kubectl logs $(kubectl get pods -n kube-system --no-headers -o custom-columns=":metadata.name" | grep vpa-updater) -n kube-system`

Drain node
-  `kubectl drain <name_of_node>`

Cordon node
- `kubectl cordon <name_of_node>`

Uncordon node
- `kubectl uncordon <name_of_node>`

Upgrade process
- Get Kubernetes version - `kubectl get nodes`
- Get OS version on node - `cat /etc/*release*`
- Change the source repository to the version to be upgraded to - https://kubernetes.io/docs/tasks/administer-cluster/kubeadm/change-package-repository/
- Get kubeadm version installed - `kubeadm version`
- Get the latest available version for upgrade - `kubeadm upgrade plan`
- Drain controlplane node - `kubectl drain controlplane`
- Determine which kubeadm version to upgrade to - https://kubernetes.io/docs/tasks/administer-cluster/kubeadm/kubeadm-upgrade/#determine-which-version-to-upgrade-to
- Upgrade kubeadm and controlplane node - https://kubernetes.io/docs/tasks/administer-cluster/kubeadm/kubeadm-upgrade/#call-kubeadm-upgrade
- Upgrade kubelet and kubectl - https://kubernetes.io/docs/tasks/administer-cluster/kubeadm/kubeadm-upgrade/#upgrade-kubelet-and-kubectl
- Upgrade worker nodes - https://kubernetes.io/docs/tasks/administer-cluster/kubeadm/kubeadm-upgrade/#upgrade-worker-nodes

Backup and restore methods
- Save this yaml file for backup - `kubectl get all --all-namespaces -o yaml > all-deploy-services.yaml`
	- To restore run `kubectl create -f ...`
- Get ETCDCTL version - `etcdctl version`
- ETCDCTL save snapshot (take backup)
	- `ETCDCTL_API=3 etcdctl snapshot save snapshot.db`
	- ETCDCTL snapshot status
		- `ETCDCTL_API=3 etcdctl snapshot status snapshot.db`
- ETCDCTL restore from backup
	- Stop kube-apiserver - `service kube-apiserver stop`
	- Restore from backup
		- `ETCDCTL_API=3 etdctl snapshot restore snapshot.db --data-dir <path_of_backup_file> (/var/lib/etcd-from-backup)`
	- `systemctl daemon-reload` and `systemctl etcd restart`
	- Start kube-apiserver start - `service kube-apiserver start`
	- Example:
		- `ETCDCTL_API=3 etcdctl snapshot save snapshot.db --endpoints=https://127.0.0.1:2379 --cacert=/etc/etcd/ca.cert --cert=/etc/etcd/etcd-server.crt --key=/etc/etcd/etcd-server`

Certificates configured for kube-api server
- Check in `/etc/kubernetes/manifests/kube-apiserver.yaml`

Certificates configured for ETCD
- Check in `/etc/kubernetes/manifests/etcd.yaml`

To view certificate details
- `openssl x509 -in <cert_file_location> -text -noout`

To list all containers using crictl
- `crictl ps -a`

To get logs of a container, when kube-apiserver is down
- `crictl logs <container_id>`

Get CSR
- `kubectl get csr`
- `kubectl get csr <csr_name> -o yaml` -> yaml format
	- Decode it by `echo "<base64encoded_certificate>" | base64 --decode`

Approve/Reject CSR
- `kubectl certificate approve <csr_name>`
- `kubectl certificate deny <csr_name>`

Delete CSR
- `kubectl delete csr <csr_name>`

Pass config, authentication to kube-apiserver/kubectl
- `curl https://my-kube-playground:6443/api/v1/pods --key admin.key --cert admin.crt --cacert ca.crt`
- `kubectl get pods --server my-kube-playground:6443 --client-key admin.key --client-certificate admin.crt --certificate-authority ca.crt`

Show current kubeconfig view
- `kubectl config view`

Show specific kubeconfig
- `kubectl config view --kubeconfig=my-custom-config`

Get current context
- `kubectl config current-context --kubeconfig=my-custom-config`

Change context
- `kubectl config use-context prod-user@production`

Set credentials in kubeconfig file
- `kubectl config set-credentials dev-user --client-cert=/../../.. --client-key=/../../.. --kube-config=/../../..`

Set custom kubeconfig as default
- Go to `vim ~/.bashrc`
- Add `export KUBECONFIG= /root/my-kube-config` (The path to file can be anything)
- Reload bash -> `source ~/.bashrc`

Get roles
- `kubectl get roles`

Create role
- `kubectl create role <role_name> --verb=list,create,delete --resource=pods`

Get role bindings
- `kubectl get rolebindings`

Create role bindings
- `kubectl create rolebinding <rolebinding_name> --role=<role_name> --user=<user_name>`

Describe role
- `kubectl describe role <role_name>`

Describe role bindings
- `kubectl describe rolebinding <role_binding_name>`

Check access of current user
- `kubectl auth can-i create deployments`
- `kubectl auth can-i delete nodes`

Check access of other users
- `kubectl auth can-i create deployments --as dev-user`
- `kubectl auth can-i create pods --as dev-user`
- `kubectl auth can-i create deployments --as dev-user --namespace test`
- `kubectl get pods --as dev-user -n blue`

Get authorization mode
- `cat /etc/kubernetes/manifests/kube-apiserver | grep authorization-mode`
- `ps -aux | grep authorization-mode`

Get ClusterRoles
- `kubectl get clusterroles`

Create ClusterRole
- `kubectl create clusterrole <cluster_role_name> --resource=persistantvolumes,storageclasses,nodes --verb=list,create,get,watch`

Describe ClusterRole
- `kubectl describe clusterrole <clusterrole_name>`

Get ClusterRoleBindings
- `kubectl get clusterrolebindings`

Create ClusterRoleBinding
- `kubectl create clusterrolebinding <cluster_role_binding_name> --user=<user_name> --clusterrole=<clusterrole_name>`

Describe ClusterRoleBinding
- `kubectl describe clusterrolebinding <clusterrolebinding_name>`

Create serviceaccount
- `kubectl create serviceaccount <serviceaacount_name>`

Get serviceaccounts
- `kubectl get serviceaccounts`

Create token
- `kubectl create token <serviceaccount_name>`

Describe serviceaccount
- `kubectl describe serviceaccount <serviceaccount_name>`

View the secret object inside the token
- `kubectl describe secret <token_name>`
- To identify the service account used on a pod
    - `kubectl get pod <pod_name> -o yaml`
    - Search for `serviceAccountName`
    - Path mounted will be `mountPath: /var/run/secrets/kubernetes.io/service-account` -> `cat` the `token` inside this path

Create docker secret
- `kubectl create secret docker-registry <secret_name> --docker-server=... --docker-username=... --docker-password=... --docker-email=...`

Check which user the container is running as 
- `kubectl exec <container_name> -- whoami`
- `kubectl exec -it <pod_name> -- whoami`

Get network policy
- `kubectl get networkpolicy`

Describe network policy
- `kubectl describe networkpolicy <networkpolicy_name>`

Get CRD
- `kubectl get crds`

Describe custom resource definition (crd)
- `kubectl describe crd <crd_name>`

Get custom object
- `kubectl get <custom_object_name>`

Delete custom object
- `kubectl delete -f flightticket.yaml`

Mount volume inside docker container
- `docker volume create <vol_name>`
- `docker run -v <vol_name>:<path_inside_the_Container_to_mount> <container_name>`
- `docker run -v <path_to_be_mounted>:<path_inside_the_Container_to_mount> <container_name>`
- `docker run --mount type=bind,source=<path_on_docker_host>,target=<path_on_container> <container_name>`

Get Persistent Volumes
- `kubectl get persistentvolume`

Describe persistent volumes
- `kubectl describe pv <pv_name>`

Get Persistent volume claims
- `kubectl get persistentvolumeclaim`

Delete Persistent volume claim
- `kubectl delete persistentvolumeclaim <pvc_name>`

Get storage class
- `kubectl get storageclass`

Describe storage class
- `kubectl describe storageclass <storageclass_name>`

Get logs from CNI weave plugin
- `kubectl logs weave-net-sg_cmb weave -n kube-system`

CNIs are installed under
- `/opt/cni/bin/`

CNI plugin configured can be found under
- `/etc/cni/net.d/`

To delete a CNI
- `kubectl delete daemonset -n kube-flannel kube-flannel-ds` - Deletes the daemon set
- `kubectl delete cm kube-flannel-cfg -n kube-flannel` - Deletes config maps
- `rm /etc/cni/net.d/10-flannel.conflist` - Config file delete

kube-proxy mode
- `kube-proxy --proxy-mode [userspace|IPTables|IPVS]`
- Identify the mode configured -> `kubectl logs -n kube-system <kube_proxy_pod_name>`

Identify IP address range for pods in a cluster
- `cat /etc/kubernetes/manifests/kube-controller-manager.yaml | grep cluster-cidr`

Identify IP address range for services in a cluster
- `cat /etc/kubernetes/manifests/kube-apiserver.yaml | grep cluster-ip-range`

Get config file used for coreDNS
- `kubectl -n kube-system describe deployments.apps coredns | grep -A2 Args | grep corefile`

ConfigMap containing coreDNS configuration
- `kubectl describe configmap coredns -n kube-system`

Get ingress
- `kubectl get ingress -A`

Create ingress resource
- `kubectl create ingress <name_of_ingress> -n <namespace> --rule="/path=servicename:port"`
    
- `kubectl create ingress ingress-pay -n critical-space --rule="/pay=pay-service:8282"`
- `kubectl create ingress ingress-test --rule="wear.my-online-store.com/wear*=wear-service:80"`

Create Ingress controller
- `kubectl create namespace ingress-nginx`
- `kubectl create configmap ingress-nginx-controller -n ingress-nginx`
- `kubectl create serviceaccount ingress-nginx -n ingress-nginx`
- `kubectl create serviceaccount ingress-nginx-admission -n ingress-nginx`
- Create the ingress controller deployment and a service for it
	- Add annotations in the service to avoid redirects

Install NGINX Gateway Fabric as Gateway Controller
1. Install the **Gateway API resources**
`kubectl kustomize "https://github.com/nginx/nginx-gateway-fabric/config/crd/gateway-api/standard?ref=v1.5.1" | kubectl apply -f -`
2. Deploy the **NGINX Gateway Fabric CRDs**
`kubectl apply -f https://raw.githubusercontent.com/nginx/nginx-gateway-fabric/v1.6.1/deploy/crds.yaml`
3. Deploy **NGINX Gateway Fabric**
`kubectl apply -f https://raw.githubusercontent.com/nginx/nginx-gateway-fabric/v1.6.1/deploy/nodeport/deploy.yaml`
4. Verify the **Deployment**
`kubectl get pods -n nginx-gateway`
5. View the `nginx-gateway` service
`kubectl get svc -n nginx-gateway nginx-gateway -o yaml`
6. Update the `nginx-gateway` service to expose ports `30080` for **HTTP** and `30081` for **HTTPS**
`kubectl patch svc nginx-gateway -n nginx-gateway --type='json' -p='[{"op": "replace", "path": "/spec/ports/0/nodePort", "value": 30080},{"op": "replace", "path": "/spec/ports/1/nodePort", "value": 30081}]'`

Switch to a namespace
- `kubectl config set-context --current --namespace=<name_of_namespace>`

Set alias and autocomplete
- `alias k=kubectl`
- https://kubernetes.io/pt-br/docs/reference/kubectl/cheatsheet/