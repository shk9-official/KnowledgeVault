
### Kubectx and Kubens - Command line Utilities

Throughout the course, you have had to work on several different namespaces in the practice lab environments. In some labs, you also had to switch between several contexts.

While this is excellent for hands-on practice, in a real “live” Kubernetes cluster implemented for production, there could be a possibility of often switching between a large number of namespaces and clusters.

This can quickly become a confusing and overwhelming task if you have to rely on `kubectl` alone.

This is where command line tools such as **kubectx** and **kubens** come into the picture.

### Reference:

[https://github.com/ahmetb/kubectx](https://github.com/ahmetb/kubectx)

### **Kubectx**

With **kubectx**, you don't have to use lengthy `kubectl config` commands to switch between contexts. This tool is particularly useful for switching between clusters in a multi-cluster environment.

#### Installation:

```
sudo git clone https://github.com/ahmetb/kubectx /opt/kubectx
sudo ln -s /opt/kubectx/kubectx /usr/local/bin/kubectx

 
```

#### Syntax:

To list all contexts:

```
kubectx

```

To switch to a new context:

```
kubectx 

```

To switch back to the previous context:

```
kubectx -

```

- To see the current context:
    

```
kubectx -c

```

### Kubens

This tool allows users to switch between namespaces quickly with a simple command.

#### Installation:

```
sudo git clone https://github.com/ahmetb/kubectx /opt/kubectx
sudo ln -s /opt/kubectx/kubens /usr/local/bin/kubens

```

#### Commands:

To switch to a new namespace:

```
kubens 

```

To switch back to the previous namespace:

```
kubens -

```


---
