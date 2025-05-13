
### Etcd

- Distributed, reliable key-value store that is simple, secure, and fast
- Key-value store example
```
"name": "John"
"age": "30"
"location": "NY"
```

**Install Etcd**

- Download binary based on OS from release pages etcd.io/etcd/releases/
- Extract - `tar -zxvf ....tar.gz`
- Run etcd service
	- $`./etcd`
	- This starts etcd service on port 2379
- Default control client which comes up with Etcd is `etcdctl`
	- $`./etcdctl set key 1 val 1`
		- This stores a key value pair
	- $`./etcdctl get key 1`
		- Retrieves a value
	- $`./etcdctl`
		- Shows all options
	- $`./etcdctl --version`
		- Shows both etcdctl version and the supported api version
	- To run etcdctl using a specific API version, (by default it is version 2)
		- $`ETCDCTL_API=3 ./etcdctl version` - by setting environment variable before running each command
		- Or set it for entire session
			- $`export ETCDCLT_API=3`
			- $`./etcdctl version`
	- In version 3
		- $`./etcdctl put key 1 val 1` - Stores / sets a key value pair
		- $`./etcdctl get key 1` - Retrieves / gets a value
- Etcd's role in Kubernetes
	- Etcd stores information about cluster such as
		- Nodes
		- Pods
		- Configurations
		- Secrets
		- Accounts
		- Roles
		- Bindings
- Every information we get from running `kubectl get` command is from etcd store.
- Any changes to cluster is updated in ecd store

**Configuring Etcd**

- Two ways to configure
	- Manually
		- Kube-apiserver should be configured with the client-url of the etcd server with port 2379
	- via Kubeadm
		- Kubeadm deploys etcd server as a pod
- $`kubectl get pods -n kube-system`
	- Lists all pods in the control plane, including the etcd server pod (etcd-master)
	- ![etcdkubeadm.png](Attachments/etcdkubeadm.png)
	- You can explore the database using etcdctl utility within the etcd-master pod.
	- $`kubectl exec etcd-master -n kube-system etcdctl get / --prefix -keys-only` - Gives all keys stored by Kubernetes
- Kubernetes stores data in a specific directory structure
	- Registry
		- Minions
		- Pods
		- ReplicaSets
		- Deployments
		- Roles
		- Secrets
- In a high availability environment, multiple etcd servers are spread across multiple master nodes
	- Configuration and settings are present in `etcd.service` file ("initial-cluster" is for high availability)


---

### Additional Etcd notes

ETCDCTL is the CLI tool used to interact with ETCD.ETCDCTL can interact with ETCD Server using 2 API versions – Version 2 and Version 3. By default it’s set to use Version 2. Each version has different sets of commands.

For example, ETCDCTL version 2 supports the following commands:

```
etcdctl backup
etcdctl cluster-health
etcdctl mk
etcdctl mkdir
etcdctl set
```

Whereas the commands are different in version 3

```
etcdctl snapshot save
etcdctl endpoint health
etcdctl get
etcdctl put
```

To set the right version of API set the environment variable ETCDCTL_API command

```
export ETCDCTL_API=3
```

When the API version is not set, it is assumed to be set to version 2. And version 3 commands listed above don’t work. When API version is set to version 3, version 2 commands listed above don’t work.

Apart from that, you must also specify the path to certificate files so that ETCDCTL can authenticate to the ETCD API Server. The certificate files are available in the etcd-master at the following path. We discuss more about certificates in the security section of this course. So don’t worry if this looks complex:

```
--cacert /etc/kubernetes/pki/etcd/ca.crt
--cert /etc/kubernetes/pki/etcd/server.crt
--key /etc/kubernetes/pki/etcd/server.key
```

So for the commands, I showed in the previous video to work you must specify the ETCDCTL API version and path to certificate files. Below is the final form:

```
kubectl exec etcd-controlplane -n kube-system -- sh -c "ETCDCTL_API=3 etcdctl get / \
  --prefix --keys-only --limit=10 / \
  --cacert /etc/kubernetes/pki/etcd/ca.crt \
  --cert /etc/kubernetes/pki/etcd/server.crt \
  --key /etc/kubernetes/pki/etcd/server.key"
```


---
