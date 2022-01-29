# Task 3

### Create pv in kubernetes

```
kubectl apply -f pv.yaml
```
   Output:
   
![](img/create_pv.png)

### Check our pv

```
kubectl get pv
```
   Output:
   
![](img/check_pv.png)

### Create pvc

```
kubectl apply -f pvc.yaml
```
   Output:
   
![](img/create_pvc.png)

### Check our output in pv

```
kubectl get pv
```
   Output:
   
![](img/check_pvc.png)

   Output is change. PV get status bound.
   
### Check pvc

   Output:
   
![](img/check_pvc_1.png)

### Apply deployment minio

```
kubectl apply -f deployment.yaml
```
   Output:
   
![](img/create_deploy.png)

### Apply svc nodeport

```
kubectl apply -f minio-nodeport.yaml
```
   Output:
   
![](img/create_nodeport.png)

   Open minikup_ip:node_port in you browser

   Output:
   
![](img/nodeport_in_browser.png)

### Apply statefulset

```
kubectl apply -f statefulset.yaml
```
   Output:
   
![](img/create_statefulset.png)

### Check pod and statefulset

```
kubectl get pod
kubectl get sts
```

   Output:
   
![](img/check_sts.png)

# Homework

   * We published minio "outside" using nodePort. Do the same but using ingress.
   
   Output:
   
![](img/ingress_minio.png)

   Output:
   
![](img/create_ingress.png)


   Output:
   
![](img/minio_port80.png)
   
   * Publish minio via ingress so that minio by ip_minikube and nginx returning hostname (previous job) by path ip_minikube/web are available at the same time.
   
   Output:
   
![](img/deploy_nginx.png)

   Output:
   
![](img/ingress_nginx.png)

   Output:
   
![](img/welcome_nginx.png)
       
   * Create deploy with emptyDir save data to mountPoint emptyDir, delete pods, check data.
   
   Output:
   
![](img/emptydir.png)

   Output:
   
![](img/apply_emptydir.png)

   Output:
   
![](img/touch_file.png)

   Output:
   
![](img/delete_pod.png)

   Output:
   
![](img/no_file_in_emptydir.png)
   
   * Optional. Raise an nfs share on a remote machine. Create a pv using this share, create a pvc for it, create a deployment. Save data to the share, delete the deployment, delete the pv/pvc, check that the data is safe.
