# Task 2

## ConfigMap & Secrets
```
kubectl create secret generic connection-string --from-literal=DATABASE_URL=postgres://connect --dry-run=client -o yaml > secret.yaml
```
   Output:
   
![](img/secret.png)
```
kubectl create configmap user --from-literal=firstname=firstname --from-literal=lastname=lastname --dry-run=client -o yaml > cm.yaml
```
   Output:
   
![](img/cm.png)

```
kubectl apply -f secret.yaml
kubectl apply -f cm.yaml
kubectl apply -f pod.yaml
```
   Output:
   
![](img/apply_files.png)

### Check env in pod

```
kubectl exec -it nginx -- bash
printenv
```
   Output:
   
![](img/printenv.png)

### Create deployment with simple application

```
kubectl apply -f nginx-configmap.yaml
kubectl apply -f deployment.yaml
```

### Get pod ip address

```
kubectl get pods -o wide
```
   Output:
   
![](img/get_pods.png)

   Try connect to pod with `curl` (`curl pod_ip_address`). What happens?
   * From you PC
   Output:
   
![](img/curl_pc.png)

   * From minikube (`minikube ssh`)
   Output:
   
![](img/curl_minikube.png)

   * From another pod (`kubectl exec -it $(kubectl get pod |awk '{print $1}'|grep web-|head -n1) bash`)
   Output:
   
![](img/curl_another_pod.png)

## Create service (ClusterIP)

   The command that can be used to create a manifest template
```
kubectl expose deployment/web --type=ClusterIP --dry-run=client -o yaml > service_template.yaml
```
   Output:
   
![](img/svc_manifest.png)

   Apply manifest
```
kubectl apply -f service_template.yaml
```
   Get service CLUSTER-IP
```
kubectl get svc
```
   Output:
   
![](img/get_svc.png)

   Try connect to service (`curl service_ip_address`). What happens?

   * From you PC
   Output:
   
![](img/curl_avc_form_pc.png)

   * From minikube (`minikube ssh`) (run the command several times)
   Output:
   
![](img/curl_svc_from_minikube.png)

   * From another pod (`kubectl exec -it $(kubectl get pod |awk '{print $1}'|grep web-|head -n1) bash`) (run the command several times)
   Output:
   
![](img/curl_svc_from_pod.png)

## NodePort
```
kubectl apply -f service-nodeport.yaml
kubectl get service
```
   Output:
   
![](img/svc_np.png)

   Note how port is specified for a NodePort service
   
### Checking the availability of the NodePort service type
```
```
   Output:
   
![](img/curl_np.png)

## Headless service
```
kubectl apply -f service-headless.yaml
```
   Output:
   
![](img/headless_svc.png)

## DNS

   Connect to any pod
```
cat /etc/resolv.conf
```
   Output:
   
![](img/resolve_file.png)

   Compare the IP address of the DNS server in the pod and the DNS service of the Kubernetes cluster.
   
   Output:
   
![](img/kube_dns.png)

   * Compare headless and clusterip Inside the pod run nslookup to normal clusterip and headless. Compare the results. You will need to create pod with dnsutils.
   Output:
   
![](img/nslookup_svc.png)

## Ingress

   Enable Ingress controller 
```
minikube addons enable ingress
```
   Output:
   
![](img/ingress_enable.png)

   Let's see what the ingress controller creates for us
```
kubectl get pods -n ingress-nginx
```
   Output:
   
![](img/get_ingress.png)

```
kubectl get pod $(kubectl get pod -n ingress-nginx|grep ingress-nginx-controller|awk '{print $1}') -n ingress-nginx -o yaml
```
   Output:
   
![](img/ingress_yaml.png)

   Create Ingress
```
kubectl apply -f ingress.yaml
curl $(minikube ip)
```
   Output:
   
![](img/ingress_round_robin.png)

## Homework

   In Minikube in namespace kube-system, there are many different pods running. Your task is to figure out who creates them, and who makes sure they are running (restores them after deletion).
   
   Output:
   
![](img/hw_1.png)

   Implement Canary deployment of an application via Ingress. Traffic to canary deployment should be redirected if you add "canary:always" in the header, otherwise it should go to regular deployment. Set to redirect a percentage of traffic to canary deployment.
   
   Output:
   
![](img/ingress_canary.png)


