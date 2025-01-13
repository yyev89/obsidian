find out public IP address:
```bash
# -q quiet mode, -O stdout output
wget -q -O - https://ipinfo.io/ip
# -s silent mode
curl -s https://api.ipify.org
```

check if service is working on port 80:
```bash
# -z scan mode, -v output mode, -w timeout
nc -zvw 3 google.com 80
# in Kubernetes pod:
kubectl exec -it my-pod -- /bin/sh
nc -zvw 3 my-service 80
```

use different versions of terraform:
```bash
tfenv install 1.0.0
tfenv use 1.0.0
```

scan Kubernetes cluster for deprecated APIs:
```bash
pluto detect-all-in-cluster --target-versions k8s=v1.32.0 -o wide 2>/dev/null
```

get logs from pods with "app=cd" in "preproduction" namespace:
```bash
kubectl stern --selector app=cd --namespace preproduction
```