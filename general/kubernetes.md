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
kubectl get namespaces[ns]
```

list resources in specific namespace:
```bash
kubectl get pods --namespace[-n] kube-system
```

list resources in all namespaces at once:
```bash
kubectl get pods --all-namespaces[-A]
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

configure kubeconfig to run all commands against one namespace:
```bash
kubectl config set-context --current --namespace <name>
```

delete namespace:
```bash
# --wait=false option to exit immidiately:
kubectl delete namespace <name>
kubectl delete -f Namespace.yaml
```

list namespace limits:
```bash
kubectl describe ns default
```

### Pod

list all pods:
```bash
kubectl get pods
# With additional info:
kubectl get pods -o wide
kubectl get pod <name> -o yaml | yq .metadata.labels
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
# For multicontainer pod:
kubectl logs --container app
```

run a remote command for the first container in the pod:
```bash
kubectl exec hello-pod -- curl localhost:8080
# Establish a sh session with the first container in a pod:
kubectl exec -it hello-pod -- sh
```

change fields on running pod via default editor:
```bash
kubectl edit pod hello-pod
```

list pods with their labels:
```bash
kubectl get pods --show-labels
# Show specific labels:
kubectl get pods --label-columns[-L] app,rel
```

add a label to a pod:
```bash
kubectl label pod kiada-canary app=kiada rel=canary
# To all pods:
kubectl label pod --all suite=kiada-suite
# Remove a label:
kubectl label pod --all suite-
```

filter output by a label:
```bash
kubectl get pods -l app=quote,rel=!canary
# Set-based selector:
kubectl get pods -l 'app in (quiz, quote)' -L app
kubectl get pods -l 'app notin (kiada)'
# List pods which have/don't have the rel label:
kubectl get pods -l rel
kubectl get pods -l '!rel'
```

list pods running on the certain node:
```bash
kubectl get pods --field-selector spec.nodeName=kind-worker
```

list pods which are not running:
```bash
kubectl get pods --field-selector status.phase!=Running
```

### ReplicaSet

**labels** are the link between RS and Pods

list all replicasets:
```bash
kubectl get replicasets[rs]
```

### Deployment

adds the concept of "rollouts" and "rollbacks", used for long-running stateless apps

list all deployments:
```bash
kubectl get deployments[deploy]
```

imperatively scale deployment to 5 replicas:
```bash
kubectl scale deploy <name> --replicas 5
```

roll the pods in the deployment one by one:
```bash
kubectl rollout restart deployment <name>
```

monitor the progress of rolling update:
```bash
kubectl rollout status deployment <name>
```

pause/resume the rollout:
```bash
kubectl rollout pause[resume] deploy <name>
```

check the history of deployments:
```bash
kubectl rollout history deployment <name>
```

imperative revert to previous version of deployment:
```bash
kubectl rollout undo deploy <name> --to-revision=1
```

undo the rollout to the previous state of replicaset:
```bash
kubectl rollout undo deployment <name>
```

add termination time for gracefull shutdown of a pod:
```yaml
spec:
	terminationGracePeriodSeconds: 40
	containers:
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
kubectl get services[svc]
```

imperativelly create service for deployment:
```bash
kubectl expose deployment svc-test --type=LoadBalancer
# Delete:
kubectl delete svc svc-test
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

### ConfigMaps, Secrets

create a ConfigMap called testmap1 with two entries from literal command-line values:
```bash
kubectl create configmap testmap1 --from-literal shortname=AOS --from-literal longname="Agents of Shield"
```

create a ConfigMap from a file called `cmfile.txt`:
```bash
kubectl create cm testmap2 --from-file cmfile.txt
```

set an environment variable from a ConfigMap entry:
```yaml
...
spec:
  containers:
  - name: kiada
    env:
    - name: INITIAL_STATUS_MESSAGE
      valueFrom:
        configMapKeyRef:
          name: kiada-config
          key: status-message
          optional: true # Pod can run even without this env set
          ...
```

inject the entire ConfigMap into environment variables:
```yaml
...
spec:
  containers:
  - name: kiada
    envFrom:
    - configMapRef:
        name: kiada-config
        optional: true
        ...
```

prevent any editing of a ConfigMap:
```yaml
immutable: true
```

generate YAML-manifest on the fly with base64-encoded values:
```bash
kubectl create secret generic my-credentials \
	--from-literal user=my-username \
	--from-literal pass=my-password \
	--dry-run=client -o yaml
```

create a new Secret called `creds`:
```bash
kubectl create secret generic creds --from-literal user=yye89 --from-literal pwd=Password123
```

get Secret's values (base-64 encoded):
```bash
kubectl get secret creds -o yaml
```

### EKS

update local kubeconfig to use EKS cluster:
```bash
aws eks update-kubeconfig --region eu-central-1 --name production-demo
```

### Events

list all events:
```bash
kubectl get events
```

list only Warnings:
```bash
kubectl get ev --field-separator type=Warning
```

get more info:
```bash
kubectl explain events
# Drill down into specifics:
kubectl explain events.type
```