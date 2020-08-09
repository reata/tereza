## Install minikube

referring to https://kubernetes.io/docs/tasks/tools/install-minikube/

minikube allows us to run kubernetes cluster in a virtual machine on your personal computer.

## Bootstrap minikube

I'm working on an ubuntu machine with Intel Core i7-8700K (6 Cores and 12 Threads) and 32 GB memory. 
And I'm using virtualbox as my VM driver of choice.

```shell script
minikube start --cpus=12 --memory=30g --vm-driver=virtualbox
```

You can check minikube status with
```shell script
minikube status
```

## Start and Stop
After bootstrap, next time to start minikube, all the settings can be omitted
```shell script
minikube start
```

Or to stop the cluster
```shell script
minikube stop
```

## Change Cluster CPU or Memory

Current cluster (VM) must be deleted first to allow new settings
```shell script
minikube delete
```

## List Docker Images Inside Cluster
```shell script
eval $(minikube -p minikube docker-env)
docker image ls
```

## SSH into VM
```shell script
minikube ssh
```
