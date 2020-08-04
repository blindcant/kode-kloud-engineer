The Nautilus DevOps team is working on a Kubernetes template to deploy a web application on the cluster. There are some requirements to create/use persistent volumes to store the application code, and the template needs to be designed accordingly. Please find more details below:

We already created a directory /mnt/data and a file index.html under the same on node01 (you need not to access node01), that location should be mounted within the container to web server's document root (keep in mind doc root can be different for Apache and Nginx web servers).

1. Create a PersistentVolume named as pv-nautilus, and set its type to local. Configure the spec as storage class should be manual, set capacity to 6Gi, set access mode to ReadWriteOnce and set host path to /mnt/data.

2. Create a PersistentVolumeClaim named as pvc-nautilus. Configure the spec as storage class should be manual, set request storage to 3Gi, set access mode to ReadWriteOnce.

3. Create a pod named as pod-nautilus, set volume as storage-nautilus, and persistent volume claim to be named as pvc-nautilus. Container name should be container-nautilus, use image httpd with latest tag only and remember to mention tag i.e httpd:latest , container port should be default port 80, mount the volume to mount path to default doc root of web server and should be named storage-nautilus.

You can check your static website, exec into the pod and use curl command, i.e curl http://localhost.

Note: The kubectl utility on jump_host has been configured to work with the kubernetes cluster.


```bash
# Source the kubectl bash completion script.
source <(kubectl completion bash)

# View cluster information
kubectl get all

# Create peristent volume yml file - https://kubernetes.io/docs/tasks/configure-pod-container/configure-persistent-volume-storage/#create-a-persistentvolume
cat > ~/pv.yml
```

```yml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-nautilus
  labels:
    type: local
spec:
  storageClassName: manual
  accessModes:
    - ReadWriteOnce
  capacity:
    storage: 6Gi
  # https://kubernetes.io/docs/concepts/storage/volumes/#local
  hostPath:
    path: "/mnt/data"

```

```bash
# Create PV and check it
kubectl create -f ~/pv.yml
kubectl get pv
kubectl describe pv

# Create persistent volume claim file - https://kubernetes.io/docs/tasks/configure-pod-container/configure-persistent-volume-storage/#create-a-persistentvolumeclaim
kubectl create -f ~/pvc.yml
```

```yml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc-nautilus
spec:
  storageClassName: manual
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 3Gi
```

```bash
# Create PVC and check it
kubectl create -f ~/pvc.yml
kubectl get pvc
kubectl describe pvc

# Create Pod using PVC / PV - https://kubernetes.io/docs/tasks/configure-pod-container/configure-persistent-volume-storage/#create-a-pod
kubectl create -f ~/pod.yml
```

```yml
apiVersion: v1
kind: Pod
metadata:
  name: pod-nautilus
spec:
  volumes:
    - name: storage-nautilus
      persistentVolumeClaim:
        claimName: pvc-nautilus
  containers:
    - name: container-nautilus
      image: httpd:latest
      ports:
        - containerPort: 80
          name: "http-server"
      volumeMounts:
        - mountPath: "/var/www/html"
          name: storage-nautilus
```

```bash
# Create PVC and check it
kubectl create -f ~/pod.yml
kubectl get pod
kubectl describe pod

# Connect to the Pod
kubectl exec --stdin --tty pod-nautilus -- /bin/bash

# Install curl
apt-get update
apt-get install curl

# Check the web server
curl http://localhost # Ok!
```
