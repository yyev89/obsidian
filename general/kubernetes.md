### Namespace

- provides a mechanism to group resources within a cluster
- there are 4 initial namespaces: default, kube-system, kube-node-lease, kube-public
- by default, namespaces DO NOT act as a network/security boundary

to see manual and all necessary fields:
```bash
kubectl explain namespace
```

list all namespaces:
```bash
kubectl get namespaces|ns
```

create namespace in the cluster from cli:
```bash
kubectl create namespace <name>
```

apply the namespace configuration to the cluster (create from file):
```bash
kubectl apply -f Namespace.yaml
```

set default (active) namespace:
```bash
kubens <name>
```

delete namespace:
```bash
kubectl delete namespace <name>
kubectl delete -f Namespace.yaml
```

### Pod

list all pods:
```bash
kubectl get pods
```

apply pod configuration:
```bash
kubectl apply -n <namespace> -f nginx-minimal.yaml
# Set port forwarding:
kubectl port-forward -n <namespace> nginx-minimal 8080:80
```

### ReplicaSet

**labels** are the link between RS and Pods

list all replicasets:
```bash
kubectl get replicasets|rs
```

### Deployment

adds the concept of "rollouts" and "rollbacks", used for long-running stateless apps

list all deployments:
```bash
kubectl get deployments|deploy
```

roll the pods in the deployment one by one:
```bash
kubectl rollout restart deployment <name>
```

undo the rollout to the previous state of replicaset:
```bash
kubectl rollout undo deployment <name>
```