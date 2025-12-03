
### Network troubleshooting

- Check the status of pods
- If the pods are in "container creating" or not in a running state, then check the logs and events of the pods
	- `kubectl logs <pod_name>`
	- `kubectl describe pod <pod_name>`
- If errors related to CNI/weave/unable to allocate IP/Failed to create sandbox
	- Install CNI like Weave https://github.com/rajch/weave#using-weave-on-kubernetes
	- https://kubernetes.io/docs/concepts/cluster-administration/addons/#networking-and-network-policy

#### **Network Plugin in Kubernetes**

_There are several plugins available, and these are some._

**1. Weave Net:**

To install,

```

kubectl apply -f

```

You can find details about the network plugins in the following documentation :

[https://kubernetes.io/docs/concepts/cluster-administration/addons/#networking-and-network-policy](https://kubernetes.io/docs/concepts/cluster-administration/addons/#networking-and-network-policy)

**2. Flannel :**

To install,

```

kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/2140ac876ef134e0ed5af15c65e414cf26827915/Documentation/kube-flannel.yml

```

_Note: As of now, flannel does not support Kubernetes network policies._

**3. Calico :**

To install,

```

curl https://docs.projectcalico.org/manifests/calico.yaml -O



_Apply the manifest using the following command._



kubectl apply -f calico.yaml

```

Calico is said to have the most advanced cni network plugin.

_Note: If there are multiple CNI configuration files in the directory, the kubelet uses the configuration file that comes first by name in lexicographic order._

#### **DNS in Kubernetes**

Kubernetes uses **CoreDNS**. **CoreDNS** is a flexible, extensible DNS server that can serve as the Kubernetes cluster DNS.

**Memory and Pods**

In large scale Kubernetes clusters, CoreDNS’s memory usage is predominantly affected by the number of Pods and Services in the cluster. Other factors include the size of the filled DNS answer cache and the rate of queries received (QPS) per CoreDNS instance.

Kubernetes resources for **coreDNS** are:

1. _a service account named_ **_coredns_**_,_
    
2. _cluster-roles named_ **_coredns_** _and_ **_kube-dns_**
    
3. _clusterrolebindings named_ **_coredns_** _and_ **_kube-dns_**_,_
    
4. _a deployment named_ **_coredns_**_,_
    
5. _a configmap named_ **_coredns_** _and a_
    
6. _service named_ **_kube-dns_**_._
    

While analyzing the coreDNS deployment, you can see that the **_Corefile plugin_** consists of an important configuration, which is defined as a **_configmap_**.

Port **53** is used for _DNS resolution_.

```
kubernetes cluster.local in-addr.arpa ip6.arpa {

   pods insecure

   fallthrough in-addr.arpa ip6.arpa

   ttl 30

}
```

This is the backend to k8s for _cluster. local and reverse domains_.

```

proxy . /etc/resolv.conf

```

Forward out of cluster domains directly to right _authoritative DNS server_.

#### Troubleshooting issues related to coreDNS

1. If you find **CoreDNS** pods in a pending state first check that the network plugin is installed.

2. coredns pods have **CrashLoopBackOff or Error state**

If you have nodes that are running SELinux with an older version of Docker, you might experience a scenario where the coredns pods are not starting. To solve that, you can try one of the following options:

a) Upgrade to a newer version of Docker.

b) Disable **SELinux.**

c) Modify the coredns deployment to set **allowPrivilegeEscalation** to _true_:

```

kubectl -n kube-system get deployment coredns -o yaml | \\

sed 's/allowPrivilegeEscalation: false/allowPrivilegeEscalation: true/g' | \\

kubectl apply -f -

```

d) Another cause for **CoreDNS** to have CrashLoopBackOff is when a **CoreDNS** Pod deployed in Kubernetes detects a loop.

There are many ways to work around this issue; some are listed here:

- Add the following to your kubelet config yaml: **_resolvConf: _** This flag tells **_kubelet_** to pass an alternate **_resolv.conf_** to Pods. For systems using **systemd-resolved**, **_/run/systemd/resolve/resolv.conf_** is typically the location of the **_“real” resolv.conf_**, although this can be different depending on your distribution.
    
- Disable the local DNS cache on host nodes, and restore **_/etc/resolv.conf_** to the original.
    
- A quick fix is to edit your **Corefile**, replacing forward **_. /etc/resolv.conf_** with the IP address of your upstream DNS, for example forward **. 8.8.8.8**. But this only fixes the issue for **CoreDNS**, **_kubelet_** will continue to forward the invalid **_resolv.conf_** to all default dnsPolicy Pods, leaving them unable to resolve DNS.
    

3. If **CoreDNS** pods and the **kube-dns** service is working fine, check the **kube-dns** service has valid **_endpoints_**.

_kubectl -n kube-system get ep kube-dns_

If there are no endpoints for the service, inspect the service and make sure it uses the correct selectors and ports.

#### **Kube-Proxy**

**kube-proxy** is a network proxy that runs on each node in the cluster. **kube-proxy** maintains _network rules on nodes_. These network rules allow network communication to the Pods from network sessions inside or outside of the cluster.

In a cluster configured with **kubeadm**, you can find **kube-proxy** as a **_daemonset_**.

**kubeproxy** is responsible for watching _services and endpoint associated with each service_. When the client is going to connect to the service using the _virtual IP_ the **kubeproxy** is responsible for _sending traffic to actual pods_.

If you run a kubectl describe ds kube-proxy -n kube-system you can see that the **kube-proxy** binary runs with the following command inside the kube-proxy container.

Command:

```

  /usr/local/bin/kube-proxy

  --config=/var/lib/kube-proxy/config.conf

  --hostname-override=$(NODE\_NAME)

```

So it fetches the configuration from a configuration file i.e., **_/var/lib/kube-proxy/config.conf_** and we can override the hostname with the node name at which the pod is running.

In the config file, we define the **clusterCIDR, kubeproxy mode, ipvs, iptables, bindaddress, kube-config** etc.

#### Troubleshooting issues related to kube-proxy

1. Check **kube-proxy** pod in the **kube-system** namespace is running.

2. Check **kube-proxy** logs.

3. Check **configmap** is correctly defined and the config file for running **kube-proxy** binary is correct.

4. **kube-config** is defined in the **config map**.

5. check **kube-proxy** is _running_ inside the container

```

\# netstat -plan | grep kube-proxy

tcp        0      0 0.0.0.0:30081           0.0.0.0:\*               LISTEN      1/kube-proxy

tcp        0      0 127.0.0.1:10249         0.0.0.0:\*               LISTEN      1/kube-proxy

tcp        0      0 172.17.0.12:33706       172.17.0.12:6443        ESTABLISHED 1/kube-proxy

tcp6       0      0 :::10256                :::\*                    LISTEN      1/kube-proxy

```

**_References:_**

Debug Service issues:

[_https://kubernetes.io/docs/tasks/debug-application-cluster/debug-service/_](https://kubernetes.io/docs/tasks/debug-application-cluster/debug-service/)

DNS Troubleshooting:

[_https://kubernetes.io/docs/tasks/administer-cluster/dns-debugging-resolution/_](https://kubernetes.io/docs/tasks/administer-cluster/dns-debugging-resolution/)


---
