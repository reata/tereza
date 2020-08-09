Deployment
============

This folder stores all the kubernetes deployment yaml for different services

```shell script
kubectl apply -f deployment/minio-deployment.yaml
kubectl port-forward deployment/minio-deployment 9000:9000
```
