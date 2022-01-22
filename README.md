# Task 1.1

### Verify kubectl installation
```
kubectl version --client
```
   Output:
   
![](img/kubectl_install.png)

### Setup autocomplete for kubectl
```
source <(kubectl completion bash) 
```
   Output:
   
![](img/kubectl_output.png)

```
minikube start --driver=virtualbox
```
   Output:
   
![](img/minikube_vbox.png)

### Get information about cluster
```
kubectl cluster-info
```
   Output: 
   
![](img/minikube_cluster_info.png)

### Get information about available nodes
```
kubectl get nodes
```
   Output:
   
![](img/minikube_nodes.png)

## Install Kubernetes Dashboard
```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.3.1/aio/deploy/recommended.yaml
```
   Output:
   
![](img/dashboard.png)

### Check kubernetes-dashboard ns
```
kubectl get pod -n kubernetes-dashboard
```
   Output:
   
![](img/dashboard_pods.png)

## Install Metrics Server
```
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```
### Update deployment
```
kubectl edit -n kube-system deployment metrics-server
```
   Output:
   
![](img/metric_server_edit.png)

## Connect to Dashboard

### Get token
   Manual
```
kubectl describe sa -n kube-system default
# copy token name
kubectl get secrets -n kube-system
kubectl get secrets -n kube-system token_name_from_first_command -o yaml
echo -n "token_from_previous_step" | base64 -d
```

### Connect to Dashboard
```
kubectl proxy
```
   Output:
   
![](img/dashboard_in_browser.png)

# Task 1.2
## Kubernetes resources introduction
```
kubectl run web --image=nginx:latest
```
   Output:
   
![](img/run_web.png)

   * take a look at created resource in cmd "kubectl get pods"
![](img/get_pods.png)

   * take a look at created resource in Dashboard
![](img/get_pods_dashboard.png)

  * take a look at created resource in cmd
```
minikube ssh
docker container ls
```
![](img/container_ls.png)

## Specification
```
kubectl explain pods.spec
```
   Output:
   
![](img/specification.png)

   Apply manifests (download from repository)
```
kubectl apply -f pod.yaml
kubectl apply -f rs.yaml
```
   Output:
   
![](img/kubectl_apply.png)

   Look at pod
```
kubectl get pod
```
   Output:
   
![](img/pod_and_rs.png)

## You can create simple manifest from cmd
```
kubectl run web --image=nginx:latest --dry-run=client -o yaml
```
   Output:
   
![](img/manifest_from_cmd.png)

# Homework
### Create a deployment nginx. Set up two replicas. Remove one of the pods, see what happens.
   Output:
   
![](img/hw_fin.png)
