
#### Create pod
```nginx-pod.yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx:1.14.2
    ports:
    - containerPort: 80
```
- `kubectl apply -f nginx-pod.yaml`
- `kubectl create -f nginx-pod.yaml`
- `kubectl create -f nginx-pod.yaml -n default`

Namespace in pod definition file
```nginx-pod.yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  namespace: <name_of_namespace>
spec:
  containers:
  - name: nginx
    image: nginx:1.14.2
    ports:
    - containerPort: 80
```
- `kubectl create -f nginx-pod.yaml` -> No need to specify `-n` when as it is specified in definition file

Labels in pod definition file
```pod-definition.yaml
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
  labels:
    app: myapp
    type: front-end
spec:
  containers:
  - name: nginx-container
    image: nginx
```

#### Create replication controller

```
apiVersion: v1
kind: ReplicationController
metadata:
  name: myapp-rc
  labels:
    app: myapp
    type: front-end
spec:
  replicas: 3
  template:
    metadata:
      name: myapp-pod
      labels:
        app: myapp
        type: front-end
    spec:
      containers:
      - name: nginx-container
        image: nginx
```
- `kubectl create -f rc-definition.yaml`


#### Create replica sets
```
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: myapp-replicaset
  labels:
    app: myapp
    type: front-end
spec:
  replicas: 3
  template:
    metadata:
      name: myapp-pod
      labels:
        app: myapp
        type: front-end
    spec:
      containers:
      - name: nginx-container
        image: nginx
  selector:
    matchLabels:
      type: front-end
```
- `kubectl create -f replicaset-defn.yaml`
- To increase or change the number of replicas in replica set
	- Change the number of replicas mentioned in the replica set definition file
    - Run `kubectl replace -f replicaset-definition.yaml`
    - Or `kubectl scale --replicas=6 -f replicaset-definition.yaml`


#### Create deployments

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp-deployment
  labels:
    tier: frontend
    app: nginx
spec:
  selector:
    matchLabels:
      env: production
  replicas: 6
  template:
    metadata:
      name: myapp-nginx
      labels:
        env: production
    spec:
      containers:
        - name: nginx-container
          image: nginx
```
- `kubectl create -f deployment-definition.yaml --record`
- `kubectl apply -f deployment-definition.yaml --record`

#### Create Namespace

```namespace-dev.yaml
apiVersion: v1
kind: Namespace
metadata:
  name: dev
```
- `kubectl create -f namespace-dev.yaml`

#### Create resource quota for namespace

```compute-quota.yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: compute-quota
  namespace: dev
spec:
  hard:
    pods: "10"
    requests.cpu: "4"
    requests.memory: 5Gi
    limits.cpu: "10"
    limits.memory: 10Gi
```
- `kubectl create -f compute-quota.yaml`

#### Create NodePort service

```service-definition.yaml
apiVersion: v1
kind: Service
metadata:
  name: myapp-service
spec:
  type: NodePort
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30004
  selector:
    app: myapp
    type: front-end
```
- `kubectl create -f service-definition.yaml`

#### Create ClusterIP service
```backend-service-definition.yaml
apiVersion: v1
kind: Service
metadata:
  name: back-end
spec:
  type: ClusterIP
  ports:
    - targetPort: 80
      port: 80
  selector:
    app: myapp
    type: back-end
```
- `kubectl create -f backend-service-definition.yaml`

#### Create LoadBalancer service

```
apiVersion: v1
kind: Service
metadata:
  name: myapp-service
spec:
  type: LoadBalancer
  ports:
    - targetPort: 80
      port: 80
      nodePort: 30008
```

#### Assign a pod to node

``` nginx-definition.yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  -  image: nginx
     name: nginx
  nodeName: node01
```
- `kubectl create -f nginx-definition.yaml`
- If a pod is already present
	- `kubectl replace -f nginx-definition.yaml --force`
	- `kubectl create -f nginx-definition.yaml --force`
	- `kubectl apply -f nginx-definition.yaml --force`

#### Assign tolerations to pod

```nginx-pod.yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  tolerations:
    - key: "app"
      operator: "Equal"
      value: "blue"
      effect: "NoSchedule"
  containers:
  - name: nginx
    image: nginx:1.14.2
    ports:
    - containerPort: 80
```

#### Node selectors for pod

```
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  nodeSelector:
    size:Large
  containers:
  - name: nginx
    image: nginx:1.14.2
    ports:
    - containerPort: 80
```

#### Node affinity

```pod-node-affinity.yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: color
            operator: In
            values:
              - blue
  containers:
  - name: nginx
    image: nginx:1.14.2
    ports:
    - containerPort: 80
```

```deployment-node-affinity.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: red
  name: red
spec:
  replicas: 2
  selector:
    matchLabels:
      app: red
  template:
    metadata:
      labels:
        app: red
    spec:
      affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
            - matchExpressions:
              - key: node-role.kubernetes.io/control-plane
                operator: Exists
      containers:
      - image: nginx
        name: nginx
```


#### Resource requests and limits

```
apiVersion: v1
kind: Pod
metadata:
  name: simple-webapp-color
  labels:
    name: simple-webapp-color
spec:
  containers:
  - name: simple-webapp-color
    image: simple-webapp-color
    ports:
      - containerPort: 8080
    resources:
      requests:
        memory: "4Gi"
        cpu: 2
      limits:
        memory: "8Gi"
        cpu: 4
```

#### Limit ranges

```limit-range-cpu.yaml
apiVersion: v1
kind: LimitRange
metadata:
  name: cpu-resource-constraint
spec:
  limits:
  - default:
      cpu: 500m
    defaultRequest:
      cpu: 500m
    max:
      cpu: "1"
    min:
      cpu: 100m
    type: Container
```

```limit-range-memory.yaml
apiVersion: v1
kind: LimitRange
metadata:
  name: memory-resource-constraint
spec:
  limits:
  - default:
      memory: 1Gi
    defaultRequest:
      memory: 1Gi
    max:
      memory: 1Gi
    min:
      memory: 500Mi
    type: Container
```
- - `kubectl apply -f limit-range-cpu.yaml --namespace=<name_of_namespace>`
- `kubectl apply -f limit-range-memory.yaml --namespace=<name_of_namespace>`

#### Resource quotas
```
apiVersion: v1
kind: ResourceQuota
metadata:
  name: my-resource-quota
spec:
  hard:
    requests.cpu: 4
    requests.memory: 4Gi
    limits.cpu: 10
    limits.memory: 10Gi
```

#### Daemon sets
```daemonset-definition.yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: monitoring-daemon
spec:
  selector:
    matchLabels:
      app: monitoring-agent
  template:
    metadata:
      labels:
        app: monitoring-agent
      spec:
        containers:
        - name: monitoring-agent
          image: monitoring-agent
```
- `kubectl create -f daemonset-definition.yaml`
- Use the --dry-run=client option and create a deployment. Change the kind to DaemonSet to create it
```
apiVersion: apps/v1
kind: DaemonSet
metadata:
  labels:
    app: elasticsearch
  name: elasticsearch
  namespace: kube-system
spec:
  selector:
    matchLabels:
      app: elasticsearch
  template:
    metadata:
      labels:
        app: elasticsearch
    spec:
      containers:
      - image: registry.k8s.io/fluentd-elasticsearch:1.20
        name: fluentd-elasticsearch
```

#### Create priority class

```priorityclass-definition.yaml
apiVersion: scheduling.k8s.io/v1
kind: PriorityClass
metadata:
  name: high-priority
value: 1000000000
description: "Priority class for mission critical pods"
```

```pod-definition.yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  labels:
    name: nginx
spec:
  containers:
  - name: nginx
    image: nginx
    ports:
      - containerPort: 8080
  priotityClassName: high-priority
```

Change default priority class value across namespace
```
apiVersion: scheduling.k8s.io/v1
kind: PriorityClass
metadata:
  name: high-priority
value: 1000000000
description: "Priority class for mission critical pods"
globalDefault: true
```

Preempt policy can be set to `Never` or `PreemptLowerPriority`
```
apiVersion: scheduling.k8s.io/v1
kind: PriorityClass
metadata:
  name: high-priority
value: 1000000000
description: "Priority class for mission critical pods"
premptionPolicy: PreemptLowerPriority
```

#### Multiple schedulers

Create scheduler yaml file
```my-scheduler-config.yaml
apiVersion: kubescheduler.config.k8s.io/v1
kind: kubeSchedulerConfiguration
profiles:
- schedulerName: my-scheduler
leaderElection:
  leaderElect: true
  resourceNamespace: kube-system
  resourceName: lock-object-my-scheduler
```

Deploy scheduler as pod
```my-custom-scheduler.yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-custom-scheduler
  namespace: kube-system
spec:
  containers:
  - command:
    - kube-scheduler
    - --address=127.0.0.1
    - --kubeconfig=/etc/kubernetes/scheduler.conf
    - --config=/etc/kubernetes/my-scheduler-config.yaml
    image: k8s.gcr.io/kube-scheduler-amd64:v1.11.3
    name: kube-scheduler
```

Check if the scheduler is deployed as pod
- `kubectl get pods -n kube-system`

Make a pod use a custom scheduler
``` nginx-pod-definition.yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  -   image: nginx
      name: nginx
  schedulerName: my-custom-scheduler
```

#### Commands and arguments

```
apiVersion: v1
kind: Pod
metadata:
  name: ubuntu-sleeper-pod
spec:
  containers:
    - name: ubuntu-sleeper
      image: ubuntu-sleeper
      command: [ "sleep2.0" ]
      args: [ "10" ]
```

Command can be specified as follows as well
```
command:
  - "sleep"
  - "20"
```

Arguments is an array
`args: [ "10", "11", "12" ]`
```
args:
  - "--color"
  - "green"
```

- `kubectl run webapp-green --image=kodekloud/webapp-color -- --color green`
    - `--` separates options for `kubectl` utility and the options as arguments

#### Environment variables

```
apiVersion: v1
kind: Pod
metadata:
  name: ubuntu-sleeper-pod
spec:
  containers:
    - name: ubuntu-sleeper
      image: ubuntu-sleeper
      command: [ "sleep2.0" ]
      args: [ "10" ]
	 ports:
	   -containerPort: 8080
	 env:
	   -name: APP_COLOR
	    value: pink
	   -name: APP_COLOR2
	    valueFrom:
	      configMapKeyRef:
	   -name: APP_COLOR3
	    valueFrom:
	      secretKeyRef:
```

#### ConfigMap
Create ConfigMap
```config-map.yaml
   apiVersion: v1
   kind: ConfigMap
   metadata:
     name: app-config
   data:
     APP_COLOR: blue
     APP_MODE: prod
```
- `kubetl create -f config-map.yaml`
Inject ConfigMap into pod
```
apiVersion: v1
kind: Pod
metadata:
  name: ubuntu-sleeper-pod
spec:
  containers:
    - name: ubuntu-sleeper
      image: ubuntu-sleeper
      command: [ "sleep2.0" ]
      args: [ "10" ]
	 ports:
	   -containerPort: 8080
     envFrom:
       - configMapRef:
           name: app-config
```
Inject single environment variable into pod
```
apiVersion: v1
kind: Pod
metadata:
  name: ubuntu-sleeper-pod
spec:
  containers:
    - name: ubuntu-sleeper
      image: ubuntu-sleeper
      command: [ "sleep2.0" ]
      args: [ "10" ]
	 ports:
	   -containerPort: 8080
     env:
	   - name: APP_COLOR
	     valueFrom:
	       configMapKeyRef:
	         name: app-config
	         key: APP_COLOR
```
Specify volumes
```
apiVersion: v1
kind: Pod
metadata:
  name: ubuntu-sleeper-pod
spec:
  containers:
    - name: ubuntu-sleeper
      image: ubuntu-sleeper
      command: [ "sleep2.0" ]
      args: [ "10" ]
	 ports:
	   -containerPort: 8080
     volumes:
       - name: app-config-volume
         configMap:
           name: app-config
```

#### Secrets
Secrets definition file
```secret-data.yaml
    apiVersion: v1
    kind: Secret
    metadata:
      name: app-secret
    data:
      DB_Host: base64_encoded_value
      DB_User: base64_encoded_value
      DB_Password: base64_encoded_value
```
- The values must be specified in encoded format in the secret-data.yaml file
    - `echo -n 'mysql' | base64`
    - `echo -n 'root' | base64`
    - `echo -n 'passwrd' | base64`
- `kubectl create -f secret-data.yaml` - Creates Kubernetes secrets

Inject secret into pod
```
apiVersion: v1
kind: Pod
metadata:
  name: ubuntu-sleeper-pod
spec:
  containers:
    - name: ubuntu-sleeper
      image: ubuntu-sleeper
      command: [ "sleep2.0" ]
      args: [ "10" ]
	 ports:
	   -containerPort: 8080
     envFrom:
       - secretRef:
           name: app-secret
```

Inject single secret into pod
```
apiVersion: v1
kind: Pod
metadata:
  name: ubuntu-sleeper-pod
spec:
  containers:
    - name: ubuntu-sleeper
      image: ubuntu-sleeper
      command: [ "sleep2.0" ]
      args: [ "10" ]
	 ports:
	   -containerPort: 8080
     env:
	   - name: DB_Password
	     valueFrom:
	       secretKeyRef:
	         name: app-secret
	         key: DB_Password
```

Read secrets from a volume
```
apiVersion: v1
kind: Pod
metadata:
  name: ubuntu-sleeper-pod
spec:
  containers:
    - name: ubuntu-sleeper
      image: ubuntu-sleeper
      command: [ "sleep2.0" ]
      args: [ "10" ]
	 ports:
	   -containerPort: 8080
     volumes:
       - name: app-secret-volume
         secret:
           name: app-secret
```

#### Multi-container pod
```multi-container-pod.yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
    - name: simple-webapp
      image: simple-webapp
      ports:
        - containerPort: 8080
    - name: log-agent
      image: log-agent
```

#### Create HPA
```
 apiVersion: autoscaling/v2
 kind: HorizontalPodAutoscaler
 metadata:
   name: my-app-hpa
 spec:
   scaleTargetRef:
     apiVersion: apps/v1
     kind: Deployment
     name: my-app
   minReplicas: 1
   maxReplicas: 10
   metrics:
   - type: Resource
     resource:
       name: cpu
       target:
         type: Utilization
         averageUtilization: 50
```
In-place resize of pods
- Set the feature flag `FEATURE_GATES=InPlacePodVerticalScaling=true`
- Specify restart policy for cpu and memory
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
        - name: my-app
          image: nginx
          resizePolicy:
            - resourceName: cpu
              restartPolicy: NotRequired
            - resourceName: memory
              restartPolicy: RestartContainer
          resources:
            requests:
              cpu: "1"
              memory: "256Mi"
            limits:
              cpu: "500m"
              memory: "512Mi"
```

#### Create VPA
- Install VPA
	- `kubectl apply -f https://github.com/kubernetes/autoscaler/releases/latest/download/vertical-pod-autoscaler.yaml`
	- Or follow instructions in https://github.com/kubernetes/autoscaler/tree/9f87b78df0f1d6e142234bb32e8acbd71295585a/vertical-pod-autoscaler
- Get VPA
	- `kubectl get pods -n kube-system | grep vpa`
- Create VPA
```
 apiVersion: autoscaling.k8s.io/v1
 kind: VerticalPodAutoscaler
 metadata:
   name: my-app-vpa
 spec:
   targetRef:
     apiVersion: apps/v1
     kind: Deployment
     name: my-app
   updatePolicy:
     updateMode: "Auto"
   resourcePolicy:
     containerPolicies:
     - containerName: "my-app"
       minAllowed:
         cpu: "250m"
       maxAllowed:
         cpu: "2"
       controlledResources: ["cpu"]
```

#### Create CSR object
https://kubernetes.io/docs/reference/kubernetes-api/authentication-resources/certificate-signing-request-v1/

```akshay-csr.yaml
apiVersion: certificates.k8s.io/v1
kind: CertificateSigningRequest
metadata:
  name: akshay
spec:
  groups:
  - system:authenticated
  request: <Paste the base64 encoded value of the CSR file>
  signerName: kubernetes.io/kube-apiserver-client
  usages:
  - client auth
```
- Create base64 encoded csr
	- `cat akshay.csr | base64 -w 0`
- Create csr - `kubectl create -f akshay-csr.yaml`

#### Kubeconfig
- under `$HOME/.kube/config`
```
--server: my-kube-playground:6443
--client-key: admin.key
--client-certificate: admin.crt
--certificate-authority: ca.crt
```
- Create a kubeconfig file
```kubeconfig.yaml
apiVersion: v1
kind: Config

current-context: my-kube-admin@my-kube-playground

clusters:
  - name: my-kube-playground
    cluster:
      certificate-authority: ca.crt
      server: https://my-kube-playground:6443
  - name: development
  - name: production
  - name: google
contexts:
  - name: my-kube-admin@my-kube-playground
    context:
      cluster: my-kube-playground
      user: mykube-admin
      namespace: finance
  - name: dev-user@google
  - name: prod-user@production
users:
  - name: my-kube-admin
    user:
      client-certificate: admin.crt
      client-key: admin.key
  - name: admin
  - name: dev-user
  - name: prod-user
```
- `kubectl create -f kubeconfig.yaml`

#### Authorization mode in kube-apiserver service

- Specify in `--authorization-mode=Node,RBAC,Webhook,AlwaysAllow,ABAC,AlwaysDeny`

#### Create Role
```developer-role.yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: developer
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["list", "get", "create", "update", "delete"]
  resourceNames: ["blue", "orange"]
- apiGroups: [""]
  resources: ["ConfigMap"]
  verbs: ["create"]
```
- `kubectl create -f developer-role.yaml`
- Edit role
	- `kubectl edit role <role_name> -n <namespace>`

#### Create RoleBinding
```devuser-developer-binding.yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: devuser-developer-binding
subjects:
- kind: User
  name: dev-user
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: developer
  apiGroup: rbac.authorization.k8s.io
```
- `kubectl create -f devuser-developer-binding.yaml`

#### Create ClusterRole
```cluster-admin-role.yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: cluster-administrator
rules:
- apiGroups: [""]
  resources: ["nodes"]
  verbs: ["list", "get", "create", "delete"]
```
- `kubectl create -f cluster-admin-role.yaml`

#### Create ClusterRoleBinding
```cluster-admin-role-binding.yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: cluster-admin-role-binding
subjects:
- kind: User
  name: cluster-admin
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: ClusterRole
  name: cluster-administrator
  apiGroup: rbac.authorization.k8s.io
```
- `kubectl create -f cluster-admin-role-binding.yaml`

#### Use non-default serviceaccount in pod

```nginx-pod.yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx:1.14.2
  serviceAccountName: <name_of_service_account>
```

#### Disable auto-mount of default service account in pod

```nginx-pod.yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx:1.14.2
  automountServiceAccountToken: false
```

#### Use different serviceaccount in deployment

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp-deployment
  labels:
    tier: frontend
    app: nginx
spec:
  selector:
    matchLabels:
      env: production
  replicas: 6
  template:
    metadata:
      name: myapp-nginx
      labels:
        env: production
    spec:
      containers:
        - name: nginx-container
          image: nginx
      serviceAccountName: <serviceaccount_name>
```

#### Docker secret inside pod definition
```nginx-pod.yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: myprivateregistry.com:5000/nginx:1.14.2
  imagePullSecrets:
  - name: <secret_name>
```

#### Security context pod level
```
apiVersion: v1
kind: Pod
metadata:
  name: web-pod
spec:
  securityContext:
    runAsUser: 1000
  containers:
    - name: ubuntu
      image: ubuntu
      command: ["sleep", "3600"]
```

#### Security context container level
```
apiVersion: v1
kind: Pod
metadata:
  name: web-pod
spec:
  containers:
    - name: ubuntu
      image: ubuntu
      command: ["sleep", "3600"]
      securityContext:
        runAsUser: 1000
        capabilities:
          add: ["MAC_ADMIN"]
```
- Check which user the container is running as 
	- `kubectl exec <container_name> -- whoami`

#### Network Policies

``` network-policy-defn.yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: db-policy
spec:
  podSelector:
    matchLabels:
      role: db  --> Applied to all pods with label "db"
  policyTypes:
  - Ingress
  ingress:
  - from:
    - podSelector:
        matchLabels:
          name: api-pod  --> Allows ingress traffic from any pod having "API-Pod" label, across all namespaces
      namespaceSelector:
        matchLabels:
          name: prod
  ports:
  - protocol: TCP
    port: 3306
```
- `kubectl create -f network-policy-defn.yaml`

Restrict access to pods in a specific namespace
```
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: db-policy
spec:
  podSelector:
    matchLabels:
      role: db  --> Applied to all pods with label "db"
  policyTypes:
  - Ingress
  ingress:
  - from:
    - podSelector:
        matchLabels:
          name: api-pod  --> Allows ingress traffic from any pod having "API-Pod" label, across all namespaces
      namespaceSelector: --> Allows ingress traffic from all pods in namespace "prod"
        matchLabels:
          name: prod
    ports:
    - protocol: TCP
      port: 3306
```

Restrict access to pods in a specific IP range
```
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: db-policy
spec:
  podSelector:
    matchLabels:
      role: db  --> Applied to all pods with label "db"
  policyTypes:
  - Ingress
  ingress:
  - from:
    - podSelector:
        matchLabels:
          name: api-pod  --> Allows ingress traffic from any pod having "API-Pod" label, across all namespaces
      namespaceSelector: --> Allows ingress traffic from all pods in namespace "prod"
        matchLabels:
          name: prod
    - ipBlock:
        cidr: 192.168.5.10/32  --> Allows ingress traffic from all pods with IP in specified range 
    ports:
    - protocol: TCP
      port: 3306
```

Egress rule
```
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: db-policy
spec:
  podSelector:
    matchLabels:
      role: db  --> Applied to all pods with label "db"
  policyTypes:
  - Ingress
  - Egress
  ingress:
  - from:
    - podSelector:
        matchLabels:
          name: api-pod  --> Allows ingress traffic from any pod having "API-Pod" label, across all namespaces      
    ports:
    - protocol: TCP
      port: 3306
  egress:
  - to:
    - ipBlock:
        cidr: 192.168.5.10/32
    ports:
    - protocol: TCP
      port: 80
```


#### Custom resource definition (CRD)

```flightticket-custom-defn.yaml
apiVersion: apiextension.k8s.io/v1
kind: CustomResourseDefinition
metadata:
  name: flighttickets.flights.com
spec:
  scope: Namespaced
  group: flights.com
  names:
    kind: FlightTicket
    singular: flightticket
    plural: flighttickets
    shortNames:
      - ft
  versions:
    - name: v1
      served: true
      storage: true
      schema:
        openAPIV3Schema:
          type: object
          properties:
            spec:
              type: object
              properties:
                from:
                  type: string
                to:
                  type: string
                number:
                  type: string
                  minimum: 1
                  maximum: 10
```

Custom resource
```
apiVersion: flights.com/v1
kind: FlightTicket
metadata:
  name: my-flight-ticket
spec:
  from: Mumbai
  to: London
  number: 2
```
- `kubectl create -f flightticket-custom-defn.yaml`

#### Volume mounting on container

```
apiVersion: v1
kind: Pod
metadata:
  name: random-number-generator
spec:
  containers:
  - image: alpine
    name: alpine
    command: ["/bin/sh", "-c"]
    args: ["shuf -i 0-100 -n 1 >> /opt/number.out;"]
    volumeMounts:
    - mountPath: /opt --> Inside container
      name: data-volume

  Volumes:
  - name: data-volume
    hostPath:
      path: /data --> In host
      type: Directory
```
- `/data` is mounted inside the container in path `/opt`
- The host volume can be AWS EBS also
```
Volumes:
- name: data-volume
  awsElasticBlockStore:
    volumeID: <vol-id>
    fsType: ext4
```

#### Persistent volumes
```pv-defn.yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-vol1
spec:
  accessModes: --> ReadOnlyMany, ReadWriteOnce, ReadWriteMany
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Retain (can be Delete, Recycle)
  capacity:
    storage: 1Gi
  hostPath:
    path: /tmp/data
```
- If using AWS EBS, replace `hostPath` to `awsElasticBlockStore`
```
awsElasticBlockStore:
  volumeID: <vol_id>
  fsType: ext4
```
- If using GCP, replace `hostPath` to `gcePersistentDisk`
 ```
 gcePersistentDisk:
    pdName: pd-disk
    fsType: ext4
```
- Creates PV - `kubectl create -f pv-defn.yaml`

#### Persistent volume claims
```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: myclaim
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 500Mi
```
- `kubectl create -f pvc-defn.yaml`

Use persistent volume claim in pod
```
apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  containers:
  - name: myfrontend
    image: nginx
    volumeMounts:
    - mountPath: /var/www/html
      name: mypd
  Volumes:
  - name: mypd
    persistentVolumeClaim:
      claimName: myclaim
```

#### Storage class

```
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: google-storage
provisioner: kubernetes.io/gce-pd
parameters:
  type: pd-standard  -> type of disk (pd-ssd)
  replication-type: none
  name: regional-pd
```
- No PV is needed
- Corresponding PVC is below
```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: myclaim
spec:
  accessModes:
    - ReadWriteOnce
  storageClassName: google-storage
  resources:
    requests:
      storage: 500Mi
```


#### Create Ingress Resource
```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: test-ingress
  namespace: critical-space
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
    - http:
        paths:
          - path: /pay
            pathType: Prefix
            backend:
              service:
                name: pay-service
                port:
                  number: 8282
```

#### Create Gateway Resource

```
apiVersion: gateway.networking.k8s.io/v1
kind: Gateway
metadata:
  name: nginx-gateway
  namespace: nginx-gateway
spec:
  gatewayClassName: nginx
  listeners:
  - name: http
    protocol: HTTP
    port: 80
    allowedRoutes:
      namespaces:
        from: All
```

#### Create http route
```
apiVersion: gateway.networking.k8s.io/v1
kind: HTTPRoute
metadata:
  name: frontend-route
spec:
  parentRefs:
  - name: nginx-gateway
    namespace: nginx-gateway
    sectionName: http
  rules:
  - matches:
    - path:
        type: PathPrefix
        value: /
    backendRefs:
    - name: frontend-svc
      port: 80
```