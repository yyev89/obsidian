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

list namespace limits:
```bash
kubectl describe ns default
```

list default hardware limits:
```bash
kubectl get limitrange limits -o yaml
```
### Pod

list all pods:
```bash
kubectl get pods
# With additional info:
kubectl get pods -o wide
```

apply pod configuration:
```bash
kubectl apply -n <namespace> -f nginx-minimal.yaml
# Set port forwarding:
kubectl port-forward -n <namespace> nginx-minimal 8080:80
```

view logs of the pod:
```bash
kubectl logs <name>
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

### Service

- serves as an internal load balancer across replicas
- uses pod labels to determine which pods to serve
- types:
1. **ClusterIP**: internal to cluster
2. **NodePort**: listens on each node in cluster
3. **LoadBalancer**: provisions external load balancer

list services in the cluster:
```bash
kubectl get services|svc
```

### DaemonSet

- runs a copy of the specified pod on all (or a specified subset of) nodes in the cluster except controllers
- useful for apps such as: cluster storage daemon, log aggregation, node monitoring

### StatefulSet

similar to Deployment, except:
- pods get sticky identity (pod-0, pod-1 etc)
- each pod mounts separate volumes
- rollout behaviour is ordered
- enables configuring workloads that require state management (e.g. primary vs read-replica for a database)
