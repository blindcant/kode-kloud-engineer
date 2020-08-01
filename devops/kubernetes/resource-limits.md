Recently some of the performance issues were observed with some applications hosted on Kubernetes cluster. The Nautilus DevOps team has observed some resources constraints where some of the applications are running out of resources like memory, cpu etc and some of the applications are consuming much resources than needed. So team has decided to add some limits for resources utilization, below you can find more details.


Create a pod named httpd-pod and a container under it named as httpd-container, use httpd image and set limits as below:

Requests: Memory: 15Mi, CPU: 1

Limits: Memory: 20Mi, CPU: 2

Note: The kubectl utility on jump_host has been configured to work with the kubernetes cluster.


```bash
# Source the kubectl bash completion script.
source <(kubectl completion bash)

# View cluster information
kubectl get all

# Create cpu limit yml file
cat > ~/cpu-limit.yml
```

```yml
apiVersion: v1
kind: LimitRange
metadata:
  name: cpu-limit-range
spec:
  limits:
  - default:
      cpu: 2 # 1 hyperthread
    defaultRequest:
      cpu: 1
    type: Container
```

```bash
# Create and inspect cpu limit object
kubectl create -f ~/cpu-limit.yml
kubectl get limits cpu-limit-range

# Create memory limit
cat > ~/mem-limit.yml
```

```yml
apiVersion: v1
kind: LimitRange
metadata:
  name: mem-limit-range
spec:
  limits:
  - default:
      memory: 20Mi # binary, base 1024
    defaultRequest:
      memory: 15Mi
    type: Container
```

```bash
# Create and inspect mem limit object
kubectl create -f ~/mem-limit.yml
kubectl get limits mem-limit-range

# Create pod yml
kubectl run --image=httpd httpd-pod --dry-run -o yaml > ~/pod.yml

# Update ~/pod.yml, container name was wrong.
vi ~/pod.yml
```

```yml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: httpd-pod
  name: httpd-pod
spec:
  containers:
  - image: httpd
    name: httpd-container
    resources:
      requests:
        cpu: 1 # 1 hyperthread
        memory: 15Mi # binary, base 1024
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
```

```bash
# Create and inspect the pod
kubectl create -f ~/pod.yml
kubectl get pods
kubectl describe pod httpd-pod
```
