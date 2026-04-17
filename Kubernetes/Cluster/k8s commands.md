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

### Cluster eksctl commands
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
