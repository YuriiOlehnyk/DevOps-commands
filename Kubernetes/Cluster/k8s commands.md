### Cluster kubectl commands
>Cluster info/nodes/pods/namespaces
```bash
kubectl cluster-info    # get info about cluster
kubectl get nodes       # lisf of nodes

kubectl get pods -A     # see list of pods and status
kubectl get pods -n <namespace> # pods in given namespace
watch kubectl pods -A   # monitoring, refresh status of nodes every 2 seconds
kubectl get pod <pod-name> -n <namespace> -o yaml | less    # manifest version in k8s
```
>Logs/describe
```bash
kubectl describe pod <pod-name> -n <namespace>
kubectl logs <pod-name> -n <namespace> 
```
>Cluster namespaces
```bash
kubectl get ns              # see namespaces
kubectl create ns <name>    # create namespace
kubectl delete ns <name>    # delete namespace
```


### Create deployment
>Create deployments and replicas with command line(not reccomended)
```bash
kubectl create deployment <name> --image <image>    # create deployment
kubectl scale deployment <name> -- replicas 3       # 3 replics of deployment
```


### Create services
Type of services: ClusterIP(default); NodePort;  ExternalName; LoadBalancer(in Clounds clusters only).
>Create services with command line 
```bash
kubectl expose deployment <nameOfDeployment> --type=<type> -- port 80 -n <namespace>  # Create service with port 80; ClusterIp is default

```


### Cluster eksctl commands(AWS)
>Create/delete cluster
```bash
eksctl create cluster --name <name of the cluster>      # create cluster, can add other parameters as well
eksctl delete cluster --name <name of the cluster>
```
>Create/delete cluster with config file
```bash
eksctl create cluster -f <my-cluster.yaml> 
eksctl delete cluster -f <my-cluster.yaml>
```
