## Install kubectl

referring to https://kubernetes.io/docs/tasks/tools/install-kubectl/

## kubectl Commands

List pods status, including those in kube-system namespace
```shell script
kubectl get pods --all-namespaces
```

List deployments
```shell script
kubectl get deployments
```

List services
```shell script
kubectl get services
```

List namespaces
```shell script
kubectl get namespaces
```

Retrieve pod status in warehouse namespace
```shell script
kubectl describe pod -n warehouse presto-worker-85d8849b4f-7bd7v
```

List configmaps
```shell script
kubectl get configmaps -n warehouse
```

See value of a secret encoded with base64
```shell script
kubectl edit secret metastore-s3-keys -n warehouse
```
