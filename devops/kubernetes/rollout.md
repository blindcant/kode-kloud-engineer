We have an application running on Kubernetes cluster using nginx web server. The Nautilus application development team has pushed some of the latest features to prod branch and those need be deployed. The Nautilus DevOps team has created an image nginx:1.18 with latest changes.


Perform a rolling update for this application and incorporate nginx:1.18 image. The deployment name is nginx-deployment

Make sure all pods are up and running after the update.

Note: The kubectl utility on jump_host has been configured to work with the kubernetes cluster.


```bash
# Check the deployments
kubectl get deployments

# Get the YAML file
kubectl get deployments nginx-deployment -o yaml > ~/deployment.yaml

# Update YAML file
cd
vi deployment.yaml
```

```yaml
...
  strategy:
    type: RollingUpdate
...
    spec:
      containers:
      - image: nginx:1.18
```

```bash
# Apply updates
kubectl apply -f ~/deployment.yaml --record=true

# Watch rollout
kubectl rollout status deployment.apps/nginx-deployment

# View rollout history
kubectl rollout history deployment.apps/nginx-deployment

# Check pods
kubectl get pods -o wide
kubectl describe pods nginx-deployment-fb65ddd4d-9gzgx
```
