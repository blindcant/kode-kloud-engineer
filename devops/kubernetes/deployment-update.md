There is an application deployed on Kubernetes cluster. Recently Nautilus application development team developed a new version of the application that needs to be deployed now. As per new updates some new changes need to be made in this existing setup. So update the deployment and service as per details mentioned below:


We already have a deployment named nginx-deployment and service named nginx-service. Some changes need to be made in this deployment and service, make sure not to delete the deployment and service.

1.) Change the service nodeport from 30008 to 30005

2.) Change replicas count from 1 to 5

3.) Add command a command: ["sh","-c","while true; do apt update; apt install -y vim php ; sleep 3; done"]

4.) Change the image from nginx:1.18 to nginx:latest

Note: The kubectl utility on jump_host has been configured to work with the kubernetes cluster.


```bash
# Source the kubectl bash completion script.
source <(kubectl completion bash)

    3  kubectl get svc nginx-service
    4  kubectl edit svc nginx-service
    kubectl describe svc nginx-service

        5  kubectl get deployments
    6  kubectl edit deployment nginx-deployment
    kubectl describe deployment nginx-deployment
```
