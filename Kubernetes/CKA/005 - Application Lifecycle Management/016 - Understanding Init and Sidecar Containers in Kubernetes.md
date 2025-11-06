
# Understanding Init and Sidecar Containers in Kubernetes

In a **multi-container Pod**, each container is expected to run a process that stays alive for the **entire lifecycle of the Pod**.

For example, in a Pod with a **web application** and a **logging agent**, both containers are expected to remain active throughout the Pod’s lifecycle. The process in the logging agent container must stay alive as long as the web application is running. If any main container fails and the Pod's `restartPolicy` is `Always` or `OnFailure`, the **entire Pod is restarted**.

---

## Pod Restart Behavior in Multi-Container Pods

It's important to understand how restarts work in **multi-container Pods**:

- If any **main container** (i.e., containers listed under `spec.containers`) exits and the Pod's `restartPolicy` is set to `Always` or `OnFailure`, **all containers in the Pod are restarted**.
- Kubernetes does not restart individual containers within a Pod. Instead, it treats the Pod as a single unit of execution and **restarts the entire Pod** if needed.

This applies only to main containers, not init containers. Init containers are always run to completion **before** main containers begin and are not restarted individually.

However, sometimes you may want to run a process that completes and exits before the main containers start. This is where **init containers** are used.

Examples include:

- A script that pulls code or binaries from a repository before the application starts.
- A script that waits for an external service (like a database) to become available.

---

## What is an Init Container?

An **init container** is a special container that runs **before the main containers** in a Pod. Each init container must **succeed (exit 0)** before the next one is started. Once all init containers complete, the regular containers start **simultaneously**.

They are configured similarly to other containers but are placed in the `initContainers` section of the Pod spec.

> If any init container fails, the entire Pod is restarted and **all init containers are rerun from the beginning**.

---

## ✅ Using Init Containers

```
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
  labels:
    app: myapp
spec:
  initContainers:
    - name: init-myservice
      image: busybox:1.31
      command: ["sh", "-c", "until nslookup myservice; do echo waiting for myservice; sleep 2; done;"]
    - name: init-mydb
      image: busybox:1.31
      command: ["sh", "-c", "until nslookup mydb; do echo waiting for mydb; sleep 2; done;"]
  containers:
    - name: myapp-container
      image: busybox:1.28
      command: ["sh", "-c", "echo The app is running! && sleep 3600"]
```

---

## Native Sidecar Containers (Kubernetes 1.33+)

Starting with Kubernetes v1.33, **sidecar containers** are natively supported. This allows sidecar containers to follow a defined lifecycle relative to the main containers in the Pod — without requiring entrypoint hacks.

### ✳️ How Native Sidecars Work

- Declared using the `restartPolicy: Always` field inside the `initContainers` block.
- Kubernetes treats such containers as **sidecars**, ensuring they:
    - Start **before** main containers.
    - Run **alongside** them.
    - **Shut down after** the main containers complete.

---

### ✅ Example: Native Sidecar Configuration

```
apiVersion: v1
kind: Pod
metadata:
  name: sidecar-example
spec:
  initContainers:
    - name: sidecar-logger
      image: busybox:1.31
      restartPolicy: Always
      command: ["sh", "-c", "while true; do echo Sidecar running; sleep 10; done"]
  containers:
    - name: main-app
      image: busybox:1.31
      command: ["sh", "-c", "echo Main app starting; sleep 60"]
```

In this setup:

- The **`sidecar-logger`** container behaves like a sidecar, though declared in `initContainers`.
- It uses `restartPolicy: Always` to stay alive throughout the Pod lifecycle.
- Kubernetes starts the sidecar first, waits for readiness, then starts the main app.

---

📖 Learn more from the official documentation:  
👉 [Kubernetes Docs: Init Containers](https://kubernetes.io/docs/concepts/workloads/pods/init-containers/)  
👉 [Kubernetes Docs: Native Sidecar Containers](https://kubernetes.io/docs/concepts/workloads/pods/sidecar-containers/)


---
