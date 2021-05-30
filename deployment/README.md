Deployment
============

This folder stores all the kubernetes deployment yaml for different services

```shell script
kubectl apply -f deployment/<app>-deployment.yaml

# visit service
minikube service <app>-service
# or alternatively forwarding the deployment port to localhost
kubectl port-forward deployment/<app>-deployment <local_port>:<deployment_port>
```
